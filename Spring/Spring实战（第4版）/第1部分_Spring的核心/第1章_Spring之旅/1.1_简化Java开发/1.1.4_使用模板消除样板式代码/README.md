# [Spring实战（第4版）](../../../../)

## [第1部分 Spring的核心](../../../)

### [第1章 Spring之旅](../../)

#### [1.1 简化Java开发](../)

##### 1.1.4 使用模板消除样板式代码

样板式代码的一个常见范例是使用JDBC访问数据库查询数据。

>代码清单1.12 许多Java API，例如JDBC，会涉及编写大量的样板式代码
```Java
public Employee getEmployeeById(long id) {
    Connection conn = null;
    PreparedStatement stmt = null;
    ResultSet rs = null;
    try {
        conn = dataSource.getConnection();
        stmt = conn.preparedStatement("select id, firstname, lastname, salary from employee where id = ?");
        stmt.setLong(1, id);
        rs = stmt.executeQuery();
        Employee employee = null;
        if (rs.next()) {
            employee = new Employee();
            employee.setId(rs.getLong("id"));
            employee.setFirstname(rs.getLong("firstname"));
            employee.setLastname(rs.getLong("lastname"));
            employee.setSalary(rs.getLong("salary"));
        }
        return employee;
    } catch (SQLException e) {
    } finally {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {}
        }
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {}
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {}
        }
    }
    return null;
}
```

Spring通过模板封装来消除样板式代码。Spring的JdbcTemplate（利用了Java5特性的JdbcTemplate实现）使得执行数据库操作时，避免传统的JDBC样板代码称为了可能。

> 程序清单1.13 模板能够让你的代码专注于自身的职责
```Java
public Employee getEmployeeById(long id) {
    
    return jdbcTemplate.queryForObject("select id, firstname, lastname, salary from employee where id = ?", 
    new RowMapper<Employee>() {
        public Employee mapRow(ResultSet rs, int rowNum) throw SQLException {
            Employee employee = new Employee();
            employee.setId(rs.getLong("id"));
            employee.setFirstname(rs.getLong("firstname"));
            employee.setLastname(rs.getLong("lastname"));
            employee.setSalary(rs.getLong("salary"));
            return employee;
        }
    }, 
    id);
        
}
```