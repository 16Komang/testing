# Submission Management System

This is a web application designed to manage invitations and submissions. It allows users to manage invitations, track submissions, and perform actions based on the submission status. The application is built using PHP, MySQL, and Bootstrap.

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

### 1. Clone the Repository

Clone this repository to your local machine or server:

bash
git clone https://your-repository-url.git
Navigate to the project directory:
cd your-repository-directory
2. Database Setup
2.1 Create the Database
Open your MySQL shell or use a tool like phpMyAdmin.

Create a new database by running the following SQL command:
CREATE DATABASE student_reference_system4;
-- Struktur dari tabel `invitations`
CREATE TABLE `invitations` (
  `id` int(11) NOT NULL,
  `user_id` int(11) NOT NULL,
  `email` varchar(100) NOT NULL,
  `token` varchar(255) NOT NULL,
  `status` enum('Pending','Accepted','Expired') DEFAULT 'Pending',
  `expires_at` timestamp NOT NULL DEFAULT (current_timestamp() + interval 7 day),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `is_used` tinyint(1) DEFAULT 0,
  `used_by` varchar(255) DEFAULT NULL,
  `subject` varchar(255) NOT NULL,
  `message` text NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Struktur dari tabel `submissions`
CREATE TABLE `submissions` (
  `id` int(11) NOT NULL,
  `invitation_id` int(11) NOT NULL,
  `relationship` text NOT NULL,
  `comments` text NOT NULL,
  `file_path` varchar(255) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `name` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Struktur dari tabel `users`
CREATE TABLE `users` (
  `id` int(11) NOT NULL,
  `username` varchar(50) NOT NULL,
  `email` varchar(100) NOT NULL,
  `password` varchar(255) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `role` enum('admin','student','writer') NOT NULL DEFAULT 'student'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Foreign Key and Auto Increment Settings
ALTER TABLE `invitations`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `token` (`token`),
  ADD KEY `user_id` (`user_id`);

ALTER TABLE `submissions`
  ADD PRIMARY KEY (`id`),
  ADD KEY `invitation_id` (`invitation_id`);

ALTER TABLE `users`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `email` (`email`);

ALTER TABLE `invitations`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=2;

ALTER TABLE `submissions`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;

ALTER TABLE `users`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=17;

ALTER TABLE `invitations`
  ADD CONSTRAINT `invitations_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE;

ALTER TABLE `submissions`
  ADD CONSTRAINT `submissions_ibfk_1` FOREIGN KEY (`invitation_id`) REFERENCES `invitations` (`id`) ON DELETE CASCADE;
Alternatively, you can import this SQL dump directly into phpMyAdmin or MySQL CLI.

2.3 Configure Database Connection
Update the db_config.php file with your database credentials:
<?php
$host = 'localhost'; // Database host, typically 'localhost'
$dbname = 'student_reference_system4'; // Database name
$username = 'root'; // Your database username
$password = ''; // Your database password

try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbname", $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
    exit;
}
3. Mailer Configuration
To enable email notifications, configure an SMTP mailer using PHP. This is optional but recommended.

3.1 Install PHPMailer
Install PHPMailer via Composer:
composer require phpmailer/phpmailer

Berikut adalah README.md dalam format Markdown yang mencakup semua instruksi yang telah disebutkan:

markdown
Salin
Edit
# Submission Management System

This is a web application designed to manage invitations and submissions. It allows users to manage invitations, track submissions, and perform actions based on the submission status. The application is built using PHP, MySQL, and Bootstrap.

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

### 1. Clone the Repository

Clone this repository to your local machine or server:

```bash
git clone https://your-repository-url.git
cd your-repository-directory

### 2. Database Setup
#### 2.1 Create the Database
    Open your MySQL shell or use a tool like phpMyAdmin.

    Create a new database by running the following SQL command:

    CREATE DATABASE student_reference_system4;
    
#### 2.2 Import the Database Schema
You can use the following SQL schema to create the required tables in your database:

-- Struktur dari tabel `invitations`
CREATE TABLE `invitations` (
  `id` int(11) NOT NULL,
  `user_id` int(11) NOT NULL,
  `email` varchar(100) NOT NULL,
  `token` varchar(255) NOT NULL,
  `status` enum('Pending','Accepted','Expired') DEFAULT 'Pending',
  `expires_at` timestamp NOT NULL DEFAULT (current_timestamp() + interval 7 day),
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `is_used` tinyint(1) DEFAULT 0,
  `used_by` varchar(255) DEFAULT NULL,
  `subject` varchar(255) NOT NULL,
  `message` text NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Struktur dari tabel `submissions`
CREATE TABLE `submissions` (
  `id` int(11) NOT NULL,
  `invitation_id` int(11) NOT NULL,
  `relationship` text NOT NULL,
  `comments` text NOT NULL,
  `file_path` varchar(255) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `name` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Struktur dari tabel `users`
CREATE TABLE `users` (
  `id` int(11) NOT NULL,
  `username` varchar(50) NOT NULL,
  `email` varchar(100) NOT NULL,
  `password` varchar(255) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `role` enum('admin','student','writer') NOT NULL DEFAULT 'student'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Foreign Key and Auto Increment Settings
ALTER TABLE `invitations`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `token` (`token`),
  ADD KEY `user_id` (`user_id`);

ALTER TABLE `submissions`
  ADD PRIMARY KEY (`id`),
  ADD KEY `invitation_id` (`invitation_id`);

ALTER TABLE `users`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `email` (`email`);

ALTER TABLE `invitations`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=2;

ALTER TABLE `submissions`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;

ALTER TABLE `users`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=17;

ALTER TABLE `invitations`
  ADD CONSTRAINT `invitations_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE;

ALTER TABLE `submissions`
  ADD CONSTRAINT `submissions_ibfk_1` FOREIGN KEY (`invitation_id`) REFERENCES `invitations` (`id`) ON DELETE CASCADE;
Alternatively, you can import this SQL dump directly into phpMyAdmin or MySQL CLI.

2.3 Configure Database Connection
Update the db_config.php file with your database credentials:

php
Salin
Edit
<?php
$host = 'localhost'; // Database host, typically 'localhost'
$dbname = 'student_reference_system4'; // Database name
$username = 'root'; // Your database username
$password = ''; // Your database password

try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbname", $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
    exit;
}

### 3. Mailer Configuration
    To enable email notifications, configure an SMTP mailer using PHP. This is optional but recommended.

#### 3.1 Install PHPMailer
    Install PHPMailer via Composer:

    composer require phpmailer/phpmailer
    
#### 3.2 Configure PHPMailer
Update the mailer configuration in the mailer_config.php file:
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
Replace yourmailserver.com, your_email@example.com, and your_password with your actual SMTP server information.

Running the Application
Start your web server:

XAMPP/WAMP: Start Apache and MySQL from the control panel.

PHP built-in server: Run the following command in the project directory:

    php -S localhost:8080
    http://localhost:8080
    Importing Data
    To import data into the application:

    You can insert records manually through the front end.
    Or, import a .csv or .sql file using phpMyAdmin or MySQL CLI.
    
#Additional Configuration
    Session Configuration: Ensure session support is enabled (call session_start() in PHP files requiring sessions).
    Error Handling: Add proper error handling for database connections and queries.
    
#Conclusion
This application provides functionality for managing submissions, invitations, and user records. By following this guide, you should be able to set up, configure, and run the app locally or on a server.
Feel free to customize the project as needed, and don't hesitate to reach out for further assistance!
