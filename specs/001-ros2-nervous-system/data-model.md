# Data Model: ROS 2 Book

This document defines the structure of the educational content. As this is a documentation project, the "data model" refers to the organization of the content itself, not a database schema.

## Main Entities

### 1. Book Module

Represents the top-level container for the educational content.

- **Attributes**:
  - `title` (string): The main title of the module. e.g., "Physical AI & Humanoid Robotics".
  - `sectionName` (string): The name for the documentation section in Docusaurus. e.g., "Module 1: The Robotic Nervous System".

### 2. Chapter

Represents a single document or page within the Book Module.

- **Attributes**:
  - `chapterNumber` (integer): The sequential number of the chapter (1, 2, 3).
  - `title` (string): The title of the chapter.
  - `content` (Markdown/MDX): The body of the chapter, written in Docusaurus-compatible format.
  - `wordCount` (integer): The approximate number of words in the chapter.

## Relationships

- A `Book Module` contains one or more `Chapters`.
- Each `Chapter` belongs to exactly one `Book Module`.

## Validation Rules

- The `Book Module` MUST contain exactly 3 chapters.
- The total `wordCount` of all chapters combined MUST be between 3,500 and 5,000 words.
- The `content` for each chapter MUST be in `.md` format.
