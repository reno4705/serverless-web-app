# Serverless Student Data Web Application

This project demonstrates a serverless web application built on AWS. It uses AWS Lambda, Amazon API Gateway, DynamoDB, Amazon S3, and CloudFront to create a dynamic student data website. Users can add new student records and view all stored student information—all without managing any underlying servers.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Deployment Steps](#deployment-steps)
  - [1. DynamoDB Setup](#1-dynamodb-setup)
  - [2. Lambda Functions](#2-lambda-functions)
  - [3. API Gateway Configuration](#3-api-gateway-configuration)
  - [4. S3 Static Website Hosting](#4-s3-static-website-hosting)
  - [5. Securing with CloudFront](#5-securing-with-cloudfront)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

This repository contains the source code for a serverless student data web application. The application lets users:
- **Add student data**: Each record includes Student ID, Name, Class, and Age.
- **View all student data**: Retrieve stored records from DynamoDB via a GET API call.

The front-end (HTML, CSS, and JavaScript) is hosted on an S3 bucket configured for static website hosting, while secure delivery over HTTPS is achieved through CloudFront. The back-end is powered by AWS Lambda functions that interact with a DynamoDB table, and Amazon API Gateway is used to expose REST endpoints.

## Architecture
![image](https://github.com/user-attachments/assets/5db6b3ec-1c6c-419c-ba89-da479c6a37a1)

The application leverages AWS serverless services:

- **Amazon S3**: Hosts static files such as `index.html` and `scripts.js`.
- **Amazon API Gateway**: Provides RESTful endpoints that trigger Lambda functions.
- **AWS Lambda**:
  - `getStudents.py`: Retrieves student records from DynamoDB.  
    :contentReference[oaicite:0]{index=0}&#8203;:contentReference[oaicite:1]{index=1}
  - `insertStudentData.py`: Inserts new student records into DynamoDB.  
    :contentReference[oaicite:2]{index=2}&#8203;:contentReference[oaicite:3]{index=3}
- **Amazon DynamoDB**: A NoSQL database that stores student data.
- **CloudFront**: Delivers the S3-hosted web content securely over HTTPS.

## Prerequisites

- An active **AWS Account**
- Basic knowledge of AWS services (S3, Lambda, API Gateway, DynamoDB, and CloudFront)
- AWS CLI or AWS Management Console access
- IAM roles with necessary permissions for Lambda functions and DynamoDB access

## Deployment Steps

### 1. DynamoDB Setup

- Create a DynamoDB table named **studentData**.
- Define the partition key (e.g., `studentid`) and include any additional keys as required.
- Adjust table settings if necessary.

### 2. Lambda Functions

Create two Lambda functions using the AWS Lambda console (or your preferred deployment tool):

- **Get Student Data Function**
  - Use the code from `getStudents.py` which scans the **studentData** table to retrieve all records.  
    :contentReference[oaicite:4]{index=4}&#8203;:contentReference[oaicite:5]{index=5}

- **Insert Student Data Function**
  - Use the code from `insertStudentData.py` which writes a new record to the **studentData** table.  
    :contentReference[oaicite:6]{index=6}&#8203;:contentReference[oaicite:7]{index=7}

Ensure both functions have the proper IAM roles to interact with DynamoDB.

### 3. API Gateway Configuration

- Create a new REST API in API Gateway.
- Define the following methods:
  - **GET Method**: Integrate with the `getStudents` Lambda function.
  - **POST Method**: Integrate with the `insertStudentData` Lambda function.
- Deploy the API to a stage (e.g., `prod`) and note the invoke URL.
- Update the API endpoint in `scripts.js` with the deployed URL.  
  :contentReference[oaicite:8]{index=8}&#8203;:contentReference[oaicite:9]{index=9}

### 4. S3 Static Website Hosting

- Create an S3 bucket to host your static web files.
- Upload `index.html` and `scripts.js` to the bucket.  
  :contentReference[oaicite:10]{index=10}&#8203;:contentReference[oaicite:11]{index=11} :contentReference[oaicite:12]{index=12}&#8203;:contentReference[oaicite:13]{index=13}
- Enable static website hosting on the bucket and set the index document (e.g., `index.html`).
- Update the bucket policy to allow public read access for the hosted files.

### 5. Securing with CloudFront

- Create a new CloudFront distribution with the S3 bucket as the origin.
- Configure CloudFront to serve content over HTTPS.
- Update the S3 bucket policy to allow CloudFront to access your objects.
- Once deployed, use the CloudFront distribution’s domain name to access your secure website.

## Project Structure

├── index.html # Static front-end webpage
├── scripts.js # JavaScript for AJAX API calls
├── getStudents.py # Lambda function for retrieving student data
└── insertStudentData.py # Lambda function for inserting student data


- **index.html**: Contains the HTML markup and basic styling for the student data interface.  
  :contentReference[oaicite:14]{index=14}&#8203;:contentReference[oaicite:15]{index=15}
- **scripts.js**: Implements AJAX calls to the API Gateway endpoints to save and retrieve student data.  
  :contentReference[oaicite:16]{index=16}&#8203;:contentReference[oaicite:17]{index=17}
- **getStudents.py** and **insertStudentData.py**: Python-based Lambda functions that interact with DynamoDB.  
  :contentReference[oaicite:18]{index=18}&#8203;:contentReference[oaicite:19]{index=19} :contentReference[oaicite:20]{index=20}&#8203;:contentReference[oaicite:21]{index=21}

## Usage

1. **Adding Student Data**:
   - Open the website hosted via CloudFront.
   - Fill in the Student ID, Name, Class, and Age in the form.
   - Click **Save Student Data** to submit. The data is sent via a POST request to API Gateway, triggering the Lambda function to insert the record.

2. **Viewing Student Data**:
   - Click **View all Students** to fetch the records.
   - The GET request retrieves the list of students from DynamoDB and displays it in a table.

## Troubleshooting

- **API Endpoint Issues**: Ensure the API Gateway URL in `scripts.js` is updated after deployment.
- **Permission Errors**: Verify that Lambda functions have the correct IAM roles to access DynamoDB.
- **S3 Access**: Check that the bucket policy allows public access (or access via CloudFront) and that static website hosting is properly enabled.
- **CloudFront**: Allow time for distribution changes to propagate. If the website is not loading over HTTPS, verify the CloudFront settings and S3 bucket policy.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
