###  本地的连接方案

配置了 MySQL 数据库的连接池，并且提供了一个通用的查询函数和一个连接状态检查函数。

```javascript
import { createPool, Pool, PoolConnection } from 'mysql2/promise';

// 数据库配置对象，包括主机、用户、密码、数据库名、连接池配置等
const dbConfig = {
  host: 'localhost',         // 数据库主机
  user: 'root',              // 数据库用户名
  password: 'XXXXXXXX',      // 数据库密码
  database: 'mysql',         // 默认连接的数据库
  waitForConnections: true,  // 是否等待连接可用时再获取
  connectionLimit: 10,       // 最大连接数限制
  queueLimit: 5,             // 等待连接的请求队列上限，0 代表无限制
};

// 初始化 MySQL 连接池，使用上面的配置
const pool: Pool = createPool(dbConfig);

// 导出数据库连接池供其他模块使用
export { pool };

/**
 * @name: query
 * @description: 执行 MySQL 查询，并返回结果
 * @param {string} sql - 要执行的 SQL 查询字符串
 * @param {any} values - 可选的查询参数，用于防止 SQL 注入
 * @return {Promise<any>} 返回查询结果，通常为查询到的行数据
 */
export async function query(sql: string, values?: any): Promise<any> {
  let connection: PoolConnection;
  try {
    // 从连接池中获取一个数据库连接
    connection = await pool.getConnection();

    // 执行 SQL 查询，并使用 `values` 进行参数化查询以防止 SQL 注入
    const [rows] = await connection.query(sql, values);

    // 查询完成后，释放连接回连接池
    connection.release();

    // 返回查询结果
    return rows;
  } catch (err) {
    // 捕获执行过程中发生的任何错误并输出到控制台
    console.error(`Error executing query: ${err}`);

    // 如果连接已获取，确保在异常情况下释放连接
    if (connection) {
      connection.release();
    }

    // 抛出错误，交由调用者处理
    throw err;
  }
}

/**
 * @name: checkMySqlConnection
 * @description: 检查 MySQL 数据库连接是否成功
 * @return {Promise<boolean>} 返回布尔值，表示是否成功连接数据库
 */
export async function checkMySqlConnection(): Promise<boolean> {
  let connection: PoolConnection;
  try {
    // 尝试从连接池获取一个连接
    connection = await pool.getConnection();

    // 如果连接成功，输出提示信息
    console.log('MySQL 数据库已成功连接!');

    // 释放连接回连接池
    connection.release();

    // 返回 true 表示连接成功
    return true;
  } catch (err) {
    // 捕获错误并输出到控制台
    console.error(`Error checking connection: ${err}`);

    // 确保即使在发生错误时也释放连接
    if (connection) {
      connection.release();
    }

    // 返回 false 表示连接失败
    return false;
  }
}
```



### 优化与考虑：

- **错误处理**：在发生异常时，`catch` 块会处理错误，并确保连接总是被释放。这避免了连接池中的连接泄漏问题。
- **参数化查询**：在 `query` 函数中，使用了 `values` 来执行参数化查询，避免了 SQL 注入攻击的风险。
- **连接池的优势**：相比于每次执行查询时都建立新的数据库连接，连接池可以复用已有的连接，提升性能，特别是在高并发的情况下。





#### 连接释放方式

使用 `finally` 块统一释放连接，确保无论是否有错误发生，连接都会被释放

在 `query` 和 `checkMySqlConnection` 函数中，连接释放是在 `try-catch-finally` 结构下手动完成的。这种方式虽然有效，但代码有一定的重复。可以通过 `finally` 语句来确保连接总是在函数结束时被释放，避免手动检查每个地方。

```javascript
export async function query(sql: string, values?: any): Promise<any> {
  let connection: PoolConnection;
  try {
    connection = await pool.getConnection();
    const [rows] = await connection.query(sql, values);
    return rows;
  } catch (err) {
    console.error(`Error executing query: ${err}`);
    throw err;
  } finally {
    // 使用 finally 确保连接一定会被释放
    if (connection) {
      connection.release();
    }
  }
}

export async function checkMySqlConnection(): Promise<boolean> {
  let connection: PoolConnection;
  try {
    connection = await pool.getConnection();
    console.log('MySQL 数据库已成功连接!');
    return true;
  } catch (err) {
    console.error(`Error checking connection: ${err}`);
    return false;
  } finally {
    if (connection) {
      connection.release();
    }
  }
}
```

#### 错误处理与日志增强

为了提升调试和维护效率，错误处理时可以为错误日志添加更多上下文信息，比如具体的 SQL 查询或参数。

```javascript
export async function query(sql: string, values?: any): Promise<any> {
  let connection: PoolConnection;
  try {
    connection = await pool.getConnection();
    const [rows] = await connection.query(sql, values);
    return rows;
  } catch (err) {
    console.error(`Error executing query: ${err}, SQL: ${sql}, Values: ${JSON.stringify(values)}`);
    throw err;
  } finally {
    if (connection) {
      connection.release();
    }
  }
}
```

#### 重用查询函数中的连接

如果应用中存在需要执行多条 SQL 语句的场景，频繁获取和释放连接可能会导致性能问题。可以考虑创建一个支持事务的查询系统，在多个 SQL 语句执行时重用一个连接。

增加一个事务功能，确保多个操作可以在同一个连接中执行



```javascript
export async function transaction(queries: { sql: string, values: any[] }[]): Promise<any> {
  let connection: PoolConnection;
  try {
    connection = await pool.getConnection();
    await connection.beginTransaction(); // 开启事务

    const results = [];
    for (const query of queries) {
      const [result] = await connection.query(query.sql, query.values);
      results.push(result);
    }

    await connection.commit(); // 提交事务
    return results;
  } catch (err) {
    if (connection) {
      await connection.rollback(); // 发生错误时回滚事务
    }
    console.error(`Transaction failed: ${err}`);
    throw err;
  } finally {
    if (connection) {
      connection.release(); // 释放连接
    }
  }
}
```

#### 参数验证和类型安全

为了防止不合法的数据传递到数据库中，可以为 SQL 参数添加类型检查或验证。在使用 TypeScript 时，可以更精确地定义输入输出类型，确保类型安全。

使用 TypeScript 的接口定义输入参数类型，确保传递的参数符合要求。

```javascript
interface QueryOptions {
  sql: string;
  values?: any[];
}

export async function query({ sql, values }: QueryOptions): Promise<any> {
  let connection: PoolConnection;
  try {
    connection = await pool.getConnection();
    const [rows] = await connection.query(sql, values);
    return rows;
  } catch (err) {
    console.error(`Error executing query: ${err}, SQL: ${sql}, Values: ${JSON.stringify(values)}`);
    throw err;
  } finally {
    if (connection) {
      connection.release();
    }
  }
}
```

#### 提高安全性 - 使用环境变量存储敏感信息

目前数据库的配置信息（如密码）是硬编码在代码中的。为了提高安全性，应该将这些敏感信息存储在环境变量中，避免泄露。

使用 `dotenv` 模块读取 `.env` 文件中的配置信息

#### 性能优化 - 使用连接池健康检查和空闲连接自动关闭

```javascript
const dbConfig = {
  host: process.env.DB_HOST || 'localhost',
  user: process.env.DB_USER || 'root',
  password: process.env.DB_PASSWORD || '',
  database: process.env.DB_NAME || 'mysql',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
  connectTimeout: 10000,  // 10秒超时
  idleTimeout: 60000,     // 1分钟的空闲超时
};
```