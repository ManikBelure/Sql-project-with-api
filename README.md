# Sql-project-with-api
import java.sql.*;

public class StudentDAO {
    private Connection connection;
    private PreparedStatement statement;
    public StudentDAO() {
        // Establish database connection
        try {
            String url = "jdbc:mysql://localhost:3306/database_name";
            String username = "your_username";
            String password = "your_password";
            connection = DriverManager.getConnection(url, username, password);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public List<Student> getPaginatedStudentDetails(int pageNumber, int pageSize) {
        List<Student> studentList = new ArrayList<>();

        try {
            // Prepare SQL query with pagination
            String sql = "SELECT * FROM students LIMIT ? OFFSET ?";
            statement = connection.prepareStatement(sql);
            statement.setInt(1, pageSize);
            statement.setInt(2, (pageNumber - 1) * pageSize);
            
            // Execute query
            ResultSet resultSet = statement.executeQuery();
            
            // Process query results
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                int totalMarks = resultSet.getInt("total_marks");
                
                // Create Student object and add it to the list
                Student student = new Student(id, name, totalMarks);
                studentList.add(student);
            }
            
            // Close resources
            resultSet.close();
            statement.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        
        return studentList;
    }
}
public List<Student> getFilteredStudentDetails(int minMarks, int maxMarks) {
    List<Student> filteredStudentList = new ArrayList<>();

    try {
        // Prepare SQL query with filtering criteria
        String sql = "SELECT * FROM students WHERE total_marks >= ? AND total_marks <= ?";
        statement = connection.prepareStatement(sql);
        statement.setInt(1, minMarks);
        statement.setInt(2, maxMarks);
        
        // Execute query
        ResultSet resultSet = statement.executeQuery();
        
        // Process query results
        while (resultSet.next()) {
            int id = resultSet.getInt("id");
            }
        }
