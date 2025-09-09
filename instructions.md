# Surya Digital Application Instructions

Thank you for applying to [Surya Digital](https://surya-digital.com). We appreciate your interest and you taking the time to go through the recruitment process with us!

Please follow instructions below exactly, read them more than once if required. A failure to follow instructions exactly will result in your application being rejected.

Please also read the [job description](https://surya-digital.com/job-details/software-engineer-fresher) before you submit your application.

To learn a bit more about how we work at Surya Digital, please read our [Employee Handbook](https://surya-digital.com/employee-handbook).

If you are interested in learning about the kinds of things we work on, read our [blog](https://surya-digital.com/blog).

This document provides step-by-step instructions for submitting your application using the API endpoints. **You must call the endpoints in the exact order listed below.**

## Base URL

```
https://sortinghat.surya-digital.in
```

## Application Flow

### Step 1: Get Reference Data

**Endpoints:** `GET /colleges/` and `GET /branches/`

Retrieve available colleges and branches for your application.

#### Get Colleges

**Request:** `GET /colleges/`

**Response (200):**

```json
[
  {
    "id": "123e4567-e89b-12d3-a456-426614174001",
    "name": "Example University"
  }
]
```

#### Get Branches

**Request:** `GET /branches/`

**Response (200):**

```json
[
  {
    "id": 1,
    "name": "Computer Science Engineering"
  }
]
```

Use the above `id`s for your college & your branch in next steps. In case your branch does not match exactly, pick the closest one.

---

### Step 2: Start Application

**Endpoint:** `POST /startApplication/`

Initiates your application, validates your GitHub username, and creates a GitHub repository for you.

**Request Body:**

```json
{
  "gitHubUserName": "your-github-username",
  "collegeId": "123e4567-e89b-12d3-a456-426614174001"
}
```

**Success Response (200):**

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000"
}
```

**Important Notes:**

- Save the returned `id` - you'll need it for all subsequent steps
- GitHub username must exist and be valid
- A private repository will be created for you in the Surya-Digital-Interviews organization with the name `<college-prefix>-<your-github-username>`
- You will be added as a collaborator to this repository
- **IMPORTANT:** You must create a file called `email.txt` at the root of this repository containing only your email address. This email will be used for your application.

---

### Step 3: Create Email File

Before proceeding to the next step, you **must** create a file called `email.txt` in the root of the GitHub repository that was created for you. This file should contain only your email address and nothing else. DO NOT CREATE ANY OTHER FILES IN THIS REPO!

**Example email.txt content:**

```
your.email@example.com
```

**Important Notes:**

- The file must be named exactly `email.txt`
- It must be at the root level of the repository (not in any subfolder)
- It should contain only your email address, with no extra text or formatting
- Make sure the email address is the same one associated with your GitHub account

---

### Step 4: Submit Student Details

**Endpoint:** `POST /studentDetails/`

Provide your personal and academic information. The email address will be automatically fetched from the `email.txt` file in your GitHub repository.

**Request Body:**

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "fullName": "Rohit Sharma",
  "phoneNumber": "+919876543210", // Phone numbers must be in this exact format
  "usn": "4NI18CS001", // Your unique student number, provided by your college
  "branchId": 1,
  "tenthPercentage": 85.5, // Must be a percentage, if you were get a GPA convert to percentage. Must be between 0 and 100.
  "twelfthPercentage": 87.25, // Must be a percentage, if you were get a GPA convert to percentage. Must be between 0 and 100.
  "btechCGPA": 8.75 // Must be a CGPA, between 0 and 10
}
```

**Success Response (200):**

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000"
}
```

**Error Responses:**

- `404`: ID not found (complete Step 1 first)

**Validation Rules:**

- Phone number: Must be of the form +911234512345
- Percentages: 0-100 with up to 2 decimal places
- CGPA: 0-10 with up to 2 decimal places
- All fields are required and cannot be empty

---

### Step 5: Upload Resume

**Endpoint:** `POST /uploadResume/`

Upload your resume as a PDF file.

**Request:** Multipart form data

- `student_id`: Your student ID from Step 2
- `file`: PDF file (max 5MB)

**Success Response (200):**

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000"
}
```

**Error Responses:**

- `404`: Student ID not found
- `413`: File too large (exceeds 5MB)

**File Requirements:**

- Must be PDF format (.pdf extension)
- Maximum size: 5MB

---

### Step 6: Submit Application

**Endpoint:** `POST /submit/`

Finalize your application with optional profile information.

**Request Body:**

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "codeChefProfileUrl": "https://www.codechef.com/users/your-username",
  "codeForcesProfileUrl": "https://codeforces.com/profile/your-username",
  "projectDetails": "Description of your projects and achievements",
  "projectDetailsUrl": "https://github.com/your-username/project-repo"
}
```

All the above fields except `id` are optional. However, you must call this API for your application to be submitted. You may call it only with `id` if the other fields are not relevant to you. Do not submit fictitious values for any of the optional fields - omit them if you do not have values to submit. Submitting fictitious values will disqualify your application.

If you have worked on a significant project, please include the details in `projectDetails`. Please note that this cannot be a straightforward thing like a library management system or a project following a YouTube tutorial. It has to be something significant that you have worked on seriously. Group projects are allowed but you must be a significant technical contributor to the project for it to be eligible.

If you are submitting a response here, we will need to see the source code of your project. A GitHub URL would be ideal, but in the event you are not able to send one, please upload a zip file with your source code and link to it in `projectDetailsUrl`. Please note that the URL must be accessible without any credentials, or authentication, or we will not be able to see it. If you have multiple urls you want to include, please put them in `projectDetails`.

**Success Response (200):**

```json
{
  "message": "Application submitted successfully",
  "id": "456e7890-e89b-12d3-a456-426614174000"
}
```

**Error Responses:**

- `404`: Application record not found

**Field Requirements:**

- URLs must start with `http://` or `https://`
- `projectDetails`: Maximum 5000 characters
- URLs: Maximum 1000 characters each

---

## Important Notes

1. **Complete all steps in order** - each step depends on the previous ones
2. **Save your student ID** from Step 2 - you'll need it for Steps 4, 5, and 6
3. **Create email.txt file** - You must create this file in your GitHub repository (Step 3) before proceeding to Step 4
4. **File upload** - Only PDF files up to 5MB are accepted for resumes

## Error Handling

All endpoints return appropriate HTTP status codes:

- `200`: Success
- `400`: Bad request (validation error)
- `422`: Bad request (validation error)
- `403`: Forbidden (rate limiting)
- `404`: Not found (invalid student ID)
- `413`: Payload too large (file size)
- `500`: Internal server error. If you get this, please write to gps@surya-digital.com with clear steps to reproduce the error.

Check the response `detail` field for specific error messages to help you resolve issues.
