# smartResumeScreener
it contains the code files and openApi schema for creating a bedrock agent 
# Resume Screening & Candidate Ranking System

## Overview

This repository contains the implementation of an automated resume screening, semantic ranking, and candidate retrieval system. It leverages AWS services including S3, Lambda, Textract, DynamoDB, Amazon Bedrock embeddings, and a Bedrock agent for HR queries.

## Features

* Automated validation of uploaded resumes (PDF, DOCX, TXT).
* Asynchronous text extraction using AWS Textract.
* Structured data parsing including personal info, skills, experience, certifications, projects, and education.
* Semantic similarity-based ranking using Amazon Bedrock embeddings.
* HR-facing agent API to fetch top candidates by match level and relevance.
* Event-driven serverless architecture for real-time processing.

## Architecture

* **S3 Buckets:** Resume uploads, processed JSON storage, invalid resumes, and job descriptions.
* **Lambda Functions:**

  * Resume Validator
  * Textract Processor
  * Resume Ranking
  * Candidate Fetcher (Bedrock agent)
* **SNS Topics:** Notifications for resume validation and Textract completion.
* **DynamoDB Tables:** ResumeUploads (raw/processed resumes), ResumeRanking (scores and match levels).
* **Amazon Bedrock:** Embedding model for semantic similarity and agent-based candidate retrieval.

## Workflow

1. HR uploads resumes to S3.
2. Resume Validator Lambda checks file type, moves invalid files, publishes SNS notification, and logs metadata.
3. Textract Processor Lambda extracts structured data from valid resumes and stores in S3/DynamoDB.
4. Resume Ranking Lambda generates embeddings, computes similarity with job description, assigns match level, and logs results in DynamoDB.
5. HR queries the Bedrock agent to retrieve top candidates filtered by match level.

## Deployment Guide

1. Create S3 buckets for uploads, processed data, invalid resumes, and job descriptions.
2. Create DynamoDB tables for resume metadata and rankings.
3. Deploy Lambda functions with appropriate triggers:

   * S3 upload for Resume Validator
   * SNS notification for Textract Processor
   * S3 processed JSON upload for Resume Ranking
   * API Gateway for Candidate Fetcher (Bedrock agent)
4. Configure SNS topics for notifications.
5. Upload job description files to the designated S3 bucket.
6. Configure IAM roles and permissions for S3, DynamoDB, SNS, Textract, and Bedrock.
7. Test the system with sample resumes and verify output in S3, DynamoDB, and agent responses.

## Usage

* Upload resumes to the `companyx-resume-uploads` bucket.
* Access the Bedrock agent API `/fetch-candidates` to query top candidates based on keywords or match level.

## Productivity Gains

* Significant reduction in manual resume validation and parsing.
* Real-time semantic ranking ensures accurate candidate-job fit.
* HR can fetch top candidates in minutes instead of hours of manual review.
