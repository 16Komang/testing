Submission Management System
This is a simple web application to manage submissions, with functionality to view, delete, and update records. It is built with PHP, MySQL, and Bootstrap. This README will guide you through setting up, running the application, and configuring the database and mailer settings.

Table of Contents
Requirements
Setup Instructions
Database Setup
Mailer Configuration
Running the Application
Requirements
PHP 7.4 or higher
MySQL or MariaDB
Composer (for dependencies management)
SMTP server (for email functionality)
Setup Instructions

1. Clone the Repository
Clone this repository to your local machine or server:

bash
Salin
Edit
git clone https://your-repository-url.git
Navigate to the project directory:

bash
Salin
Edit
cd your-repository-directory

2. Database Setup
You need to create a MySQL database and import the schema to set up the submissions table.

2.1 Create the Database
Open your MySQL shell or use a tool like phpMyAdmin.
Create a new database by running the following SQL command:
sql
Salin
Edit
CREATE DATABASE submissions_db;
2.2 Import the Database Schema
You will need to create the table structure. Create a SQL file (e.g., database_schema.sql) with the following content and import it into your database:
sql
Salin
Edit
CREATE TABLE submissions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    relationship VARCHAR(255) NOT NULL,
    comments TEXT,
    file_path VARCHAR(255) NOT NULL
);
Alternatively, you can run the SQL commands directly in phpMyAdmin or MySQL CLI.

2.3 Configure Database Connection
In the file db_config.php, update the following database connection details:
php
Salin
Edit
<?php
$host = 'localhost'; // Database host, typically 'localhost'
$dbname = 'submissions_db'; // Database name
$username = 'root'; // Your database username
$password = ''; // Your database password

try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbname", $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
    exit;
}
?>

3. Mailer Configuration
For sending notifications or alerts, you can configure an SMTP mailer using PHP. If you want to integrate email functionality, you can use PHPMailer or any other mailer library.

3.1 Install PHPMailer
To install PHPMailer via Composer, run the following command:

bash
Salin
Edit
composer require phpmailer/phpmailer
3.2 Configure PHPMailer
Update the mailer configuration in the config or a specific mailer settings file (e.g., mailer_config.php):

php
Salin
Edit
use PHPMailer\PHPMailer\PHPMailer;
use PHPMailer\PHPMailer\Exception;

require 'vendor/autoload.php';

$mail = new PHPMailer(true);
try {
    //Server settings
    $mail->isSMTP();                                            // Send using SMTP
    $mail->Host       = 'smtp.yourmailserver.com';                // Set the SMTP server to send through
    $mail->SMTPAuth   = true;                                     // Enable SMTP authentication
    $mail->Username   = 'your_email@example.com';                // SMTP username
    $mail->Password   = 'your_password';                          // SMTP password
    $mail->SMTPSecure = PHPMailer::ENCRYPTION_STARTTLS;           // Enable TLS encryption
    $mail->Port       = 587;                                      // TCP port to connect to

    //Recipients
    $mail->setFrom('your_email@example.com', 'Mailer');
    $mail->addAddress('recipient@example.com');                   // Add a recipient

    // Content
    $mail->isHTML(true);                                          // Set email format to HTML
    $mail->Subject = 'Submission Notification';
    $mail->Body    = 'Your submission has been successfully processed.';

    $mail->send();
    echo 'Message has been sent';
} catch (Exception $e) {
    echo "Message could not be sent. Mailer Error: {$mail->ErrorInfo}";
}
Make sure to replace yourmailserver.com, your_email@example.com, and your_password with your actual mail server information.

Running the Application
1. Start Your Web Server
You can use XAMPP, WAMP, or any other LAMP stack to run PHP applications.
Make sure your server is running, and the Document Root is set to the directory where your application resides.
If you're using XAMPP:

Start Apache and MySQL from the XAMPP Control Panel.
If you're using PHP's built-in server:

From your project directory, run:
bash
Salin
Edit
php -S localhost:8080
Visit http://localhost:8080 in your browser to view the app.

Importing Data
To import submissions data into the application:

Insert records manually through your app's front-end.
Alternatively, you can import a .csv or .sql file using phpMyAdmin or MySQL CLI.
Additional Configuration
Session Configuration: If you're using sessions for login and roles, ensure your server has session support enabled (session_start() should be called in each PHP file that requires it).

Error Handling: Add proper error handling in your db_config.php and other files to catch database connection issues or invalid queries.

Conclusion
This application is designed to manage submission records with CRUD operations, and it also includes features for deletion and email notifications. By following the steps in this README, you should be able to set up the database, configure email notifications, and run the app locally or on a server.

Let me know if you need further assistance!
