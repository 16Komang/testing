# Submission Management System

This is a simple web application to manage submissions, with functionality to view, delete, and update records. It is built with PHP, MySQL, and Bootstrap. This README will guide you through setting up, running the application, and configuring the database and mailer settings.

---

## Table of Contents

1. [Requirements](#requirements)
2. [Setup Instructions](#setup-instructions)
    - [Clone the Repository](#1-clone-the-repository)
    - [Database Setup](#2-database-setup)
    - [Mailer Configuration](#3-mailer-configuration)
3. [Running the Application](#running-the-application)
4. [Importing Data](#importing-data)
5. [Additional Configuration](#additional-configuration)
6. [Conclusion](#conclusion)

---

## Requirements

To run this application, ensure your system meets the following requirements:

- **PHP** 7.4 or higher
- **MySQL** or **MariaDB**
- **Composer** (for dependency management)
- **SMTP server** (for email functionality, optional)

---

## Setup Instructions

### Clone the Repository

Clone this repository to your local machine or server:

```bash
git clone https://your-repository-url.git
```

Navigate to the project directory:

```bash
cd your-repository-directory
```

---

### Database Setup

#### Create the Database

1. Open your MySQL shell or use a tool like phpMyAdmin.
2. Create a new database by running the following SQL command:

## Database Structure

### Overview
The database consists of three tables: `users`, `invitations`, and `submissions`. Below are the details of their structure and relationships:

---

### 1. `users` Table
This table stores information about the users of the system.

**Columns:**
- `id` (INT, Primary Key, Auto Increment): Unique identifier for each user.
- `username` (VARCHAR(50), NOT NULL): Username of the user.
- `email` (VARCHAR(100), NOT NULL, UNIQUE): Email address of the user.
- `password` (VARCHAR(255), NOT NULL): Password (hashed) of the user.
- `created_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP): Timestamp of when the user record was created.
- `role` (ENUM('admin', 'student', 'writer'), DEFAULT 'student', NOT NULL): Role assigned to the user.

---

### 2. `invitations` Table
This table manages invitations sent to users.

**Columns:**
- `id` (INT, Primary Key, Auto Increment): Unique identifier for each invitation.
- `user_id` (INT, Foreign Key, NOT NULL): References `users.id`.
- `email` (VARCHAR(100), NOT NULL): Email address where the invitation was sent.
- `token` (VARCHAR(255), UNIQUE, NOT NULL): Unique token for the invitation.
- `status` (ENUM('Pending', 'Accepted', 'Expired'), DEFAULT 'Pending'): Current status of the invitation.
- `expires_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP + INTERVAL 7 DAY, NOT NULL): Expiration date of the invitation.
- `created_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP, NOT NULL): Timestamp of when the invitation was created.
- `is_used` (TINYINT(1), DEFAULT 0): Indicates whether the invitation has been used.
- `used_by` (VARCHAR(255)): User who used the invitation.
- `subject` (VARCHAR(255), NOT NULL): Subject of the invitation.
- `message` (TEXT, NOT NULL): Message content of the invitation.

**Relationships:**
- `user_id` is a foreign key referencing `users.id`.

---

### 3. `submissions` Table
This table tracks submissions made through invitations.

**Columns:**
- `id` (INT, Primary Key, Auto Increment): Unique identifier for each submission.
- `invitation_id` (INT, Foreign Key, NOT NULL): References `invitations.id`.
- `relationship` (TEXT, NOT NULL): Relationship description related to the submission.
- `comments` (TEXT, NOT NULL): Comments provided in the submission.
- `file_path` (VARCHAR(255), NOT NULL): Path to the file submitted.
- `created_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP, NOT NULL): Timestamp of when the submission was created.
- `name` (VARCHAR(255)): Name of the submitter.

**Relationships:**
- `invitation_id` is a foreign key referencing `invitations.id`.

---

### Foreign Key Constraints
- **`invitations.user_id`**: References `users.id` with `ON DELETE CASCADE`.
- **`submissions.invitation_id`**: References `invitations


Alternatively, you can run the SQL commands directly in phpMyAdmin or MySQL CLI.

#### Configure Database Connection

Update the `db_config.php` file with your database credentials:

```php
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
```

---

### Mailer Configuration

To enable email notifications, configure an SMTP mailer using PHP. This is optional but recommended.

#### Install PHPMailer

Install PHPMailer via Composer:

```bash
composer require phpmailer/phpmailer
```

#### Configure PHPMailer

Update the mailer configuration in the `mailer_config.php` file:

```php
use PHPMailer\PHPMailer\PHPMailer;
use PHPMailer\PHPMailer\Exception;

require 'vendor/autoload.php';

$mail = new PHPMailer(true);
try {
    // Server settings
    $mail->isSMTP();                                            // Send using SMTP
    $mail->Host       = 'smtp.yourmailserver.com';              // Set the SMTP server to send through
    $mail->SMTPAuth   = true;                                   // Enable SMTP authentication
    $mail->Username   = 'your_email@example.com';               // SMTP username
    $mail->Password   = 'your_password';                       // SMTP password
    $mail->SMTPSecure = PHPMailer::ENCRYPTION_STARTTLS;         // Enable TLS encryption
    $mail->Port       = 587;                                   // TCP port to connect to

    // Recipients
    $mail->setFrom('your_email@example.com', 'Mailer');
    $mail->addAddress('recipient@example.com');                // Add a recipient

    // Content
    $mail->isHTML(true);                                       // Set email format to HTML
    $mail->Subject = 'Submission Notification';
    $mail->Body    = 'Your submission has been successfully processed.';

    $mail->send();
    echo 'Message has been sent';
} catch (Exception $e) {
    echo "Message could not be sent. Mailer Error: {$mail->ErrorInfo}";
}
```

Replace `yourmailserver.com`, `your_email@example.com`, and `your_password` with your actual SMTP server information.

---

## Running the Application

1. Start your web server:
   - **XAMPP/WAMP**: Start Apache and MySQL from the control panel.
   - **PHP built-in server**: Run the following command in the project directory:

     ```bash
     php -S localhost:8080
     ```

2. Open your browser and navigate to:

   ```
   http://localhost:8080
   ```

---

## Importing Data

To import submissions data:

- Insert records manually through the app's front end.
- Import a `.csv` or `.sql` file using phpMyAdmin or MySQL CLI.

---

## Additional Configuration

- **Session Configuration**: Ensure session support is enabled (call `session_start()` in PHP files requiring sessions).
- **Error Handling**: Add proper error handling for database connections and queries.

---

## Conclusion

This application provides basic CRUD functionality for managing submission records, with optional email notifications. By following this guide, you should be able to set up, configure, and run the app locally or on a server.

Feel free to customize the project as needed, and don't hesitate to reach out for further assistance!
