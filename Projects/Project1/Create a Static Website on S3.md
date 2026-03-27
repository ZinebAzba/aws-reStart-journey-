
# Keto Life Restaurant — Static Website Project  
### AWS re/Start Cohort Team Project (4 Members)

Keto Life Restaurant is a collaborative static website created by four members of the AWS re/Start cohort. The goal was to design a simple, visually appealing website and deploy it using Amazon S3 Static Website Hosting, applying real AWS cloud concepts such as storage, permissions, and public hosting.

---

## Project Overview

The website was built using HTML, CSS, and images, then hosted on Amazon S3 as a publicly accessible static site.  
The final site is available through the S3 website endpoint.

http://keto-life-restuarant.s3-website-us-west-2.amazonaws.com
---

## Team Members

This project was completed by a group of four AWS re/Start learners, each contributing to design, coding, AWS configuration, and deployment.

---

## Objectives

- Build a static website collaboratively  
- Host it using Amazon S3 Static Website Hosting  
- Configure bucket permissions and public access  
- Validate deployment using the S3 website endpoint  
- Apply AWS concepts learned in the program  

---

## Technologies & AWS Services Used

- HTML / CSS  
- Amazon S3 (Static Website Hosting)  
- Bucket Policies  
- Public Access Configuration  
- AWS Console  

---

## Deployment Steps

### 1. Website Development
- Designed the layout and theme: Keto Life Restaurant  
- Created the HTML structure and CSS styling  
- Selected and optimized images for performance  
- Ensured clean folder structure for deployment  

---

### 2. S3 Bucket Creation
- Created a bucket named:  
  `keto-life-restuarant`  
- Region: us-west-2  
- Disabled “Block all public access” (required for static hosting)  

---

### 3. Static Website Hosting Configuration
Enabled static website hosting and set:

- Index document: `index.html`  

---

### 4. Uploading Website Files
Uploaded all project assets:

- `index.html`  
- `styles.css`  
- Images folder   

Verified correct MIME types and object paths.

---

### 5. Bucket Policy for Public Access
Added a policy to allow public read access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::keto-life-restuarant/*"
    }
  ]
}
```

---

### 6. Testing the Website
Accessed the site using the S3 static hosting endpoint:

```
http://keto-life-restuarant.s3-website-us-west-2.amazonaws.com
```

Verified:

- Page loads correctly  
- Images display  
- CSS styling applied  
- Navigation works  

---

## Final Result

A fully deployed, publicly accessible static website hosted on Amazon S3, showcasing:

- Clean UI  
- Responsive layout  
- Visual content  
- Fast loading via S3 hosting  

This project demonstrates practical cloud deployment skills and effective teamwork.

---

## Skills Demonstrated

- AWS S3 static website hosting  
- Bucket policy configuration  
- Public access and permissions  
- Front-end development (HTML/CSS)  
- Cloud deployment troubleshooting  
- Team collaboration  
- Understanding AWS regions, endpoints, and object storage  
```

---
## Challenges and Notes

- Images must be saved in `.jpg` format.
- Folder and file names must follow a strict naming convention (lowercase, no spaces).
- Used the **Move** option in the **S3 bucket console** to reorganize files into the correct folder structure.
- All images should be stored in a dedicated folder to ensure correct execution and clean organization.
- Deployment was initially blocked due to a misspelled S3 bucket name, which caused access and hosting errors.
