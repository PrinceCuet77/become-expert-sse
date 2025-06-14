- [Amazon S3](#amazon-s3)
  - [Exam KeyNotes](#exam-keynotes)
  - [Introduction](#introduction)
    - [Buckets](#buckets)
    - [Objects](#objects)

# Amazon S3

## Exam KeyNotes

- A

## Introduction

- Amazon S3 is one of the main building blocks of AWS
- It’s advertised as **infinitely scaling** storage
- Many websites use Amazon S3 as a backbone
- Many AWS services use Amazon S3 as an integration as well
- Usecase:
  - Backup and storage
  - Disaster Recovery
  - Archive
  - Hybrid Cloud storage
  - Application hosting
  - Media hosting
  - Data lakes & big data analytics
  - Software delivery
  - Static website

### Buckets

- Amazon S3 stores files in buckets
- Amazon S3 allows people to store objects (files) in “buckets” (directories)
- Buckets must have a globally unique name (across all regions all accounts)
- Buckets are defined at the region level
- S3 looks like a global service but buckets are created in a region
- Naming convention
  - No uppercase, No underscore
  - `3-63` characters long
  - Not an IP
  - Must start with lowercase letter or number
  - Must NOT start with the prefix `xn--`
  - Must NOT end with the suffix `-s3alias`

![Bucket](photo/bucket.png)

### Objects

- Objects (files) have a Key
- The key is the FULL path:
  - `s3://my-bucket/my_file.txt`
  - `s3://my-bucket/my_folder1/another_folder/my_file.txt`
- The key is composed of prefix + object name
  - `s3://my-bucket/my_folder1/another_folder/my_file.txt`
- There’s no concept of **directories** within buckets (although the UI will trick you to think otherwise)
- Just keys with very long names that contain slashes (`/`)
- Object values are the content of the body:
  - Max. Object Size is `5TB` (`5000GB`)
  - If uploading more than `5GB`, must use **multi-part upload**
- Metadata (list of text key / value pairs – system or user metadata)
- Tags (Unicode key / value pair – up to 10) – useful for security / lifecycle
- Version ID (if versioning is enabled)

![Object](photo/object.png)

- After uploading any image, the public URL is not working
- As, AWS added a S3 pre-signed URL after the public URL
- The URL contains a signature that verifies that I am the one making the request & credentials on it
- As my credentials are encoded in this URL, so AWS will allow me
- So public URL is not working but pre-signed URL with my own credentials works as this URL is only for me
- 