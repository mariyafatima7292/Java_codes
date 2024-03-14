import java.sql.*;

public class CRUDExample {
    // JDBC URL, username, and password of MySQL server
    private static final String JDBC_URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String USERNAME = "username";
    private static final String PASSWORD = "password";

    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD)) {
            // CREATE
            String insertQuery = "INSERT INTO users (name, email) VALUES (?, ?)";
            try (PreparedStatement insertStatement = connection.prepareStatement(insertQuery)) {
                insertStatement.setString(1, "John Doe");
                insertStatement.setString(2, "john@example.com");
                insertStatement.executeUpdate();
                System.out.println("Record inserted successfully.");
            }

            // READ
            String selectQuery = "SELECT * FROM users";
            try (Statement statement = connection.createStatement();
                 ResultSet resultSet = statement.executeQuery(selectQuery)) {
                while (resultSet.next()) {
                    System.out.println("Name: " + resultSet.getString("name") + ", Email: " + resultSet.getString("email"));
                }
            }

            // UPDATE
            String updateQuery = "UPDATE users SET email = ? WHERE name = ?";
            try (PreparedStatement updateStatement = connection.prepareStatement(updateQuery)) {
                updateStatement.setString(1, "john.doe@example.com");
                updateStatement.setString(2, "John Doe");
                int rowsUpdated = updateStatement.executeUpdate();
                System.out.println(rowsUpdated + " record(s) updated successfully.");
            }

            // DELETE
            String deleteQuery = "DELETE FROM users WHERE name = ?";
            try (PreparedStatement deleteStatement = connection.prepareStatement(deleteQuery)) {
                deleteStatement.setString(1, "John Doe");
                int rowsDeleted = deleteStatement.executeUpdate();
                System.out.println(rowsDeleted + " record(s) deleted successfully.");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
