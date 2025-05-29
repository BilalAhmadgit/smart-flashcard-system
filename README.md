Smart Flashcard System Backend
A Flask-based REST API for a smart flashcard system that automatically classifies flashcards by subject using intelligent keyword matching and rule-based classification.

Features
Automatic Subject Classification: Automatically determines the subject of flashcards based on question and answer content
Multiple Subject Support: Supports Physics, Chemistry, Biology, Mathematics, History, Geography, Literature, Computer Science, and General categories
Intelligent Mixed Batching: Returns balanced flashcard batches across different subjects
RESTful API: Clean REST endpoints for all operations
SQLite Database: Lightweight database for data persistence
Error Handling: Comprehensive error handling and validation
Installation
Prerequisites
Python 3.7 or higher
pip (Python package installer)
Setup Instructions
Clone the repository
bash
git clone https://github.com/yourusername/smart-flashcard-system.git
cd smart-flashcard-system
Create a virtual environment (recommended)
bash
python -m venv flashcard_env

# On Windows
flashcard_env\Scripts\activate

# On macOS/Linux
source flashcard_env/bin/activate
Install dependencies
bash
pip install -r requirements.txt
Run the application
bash
python app.py
The server will start on http://localhost:5000

API Endpoints
1. Add Flashcard
POST /flashcard

Adds a new flashcard with automatic subject classification.

Request Body:

json
{
  "student_id": "stu001",
  "question": "What is Newton's Second Law?",
  "answer": "Force equals mass times acceleration"
}
Response:

json
{
  "message": "Flashcard added successfully",
  "subject": "Physics",
  "flashcard_id": 1
}
2. Get Flashcards by Subject
GET /flashcards/<subject>

Retrieves flashcards for a specific subject.

Query Parameters:

limit (optional): Number of flashcards to return (1-100, default: 10)
student_id (optional): Filter by specific student
Example: GET /flashcards/Physics?limit=5&student_id=stu001

Response:

json
{
  "subject": "Physics",
  "count": 2,
  "flashcards": [
    {
      "id": 1,
      "student_id": "stu001",
      "question": "What is Newton's Second Law?",
      "answer": "Force equals mass times acceleration",
      "subject": "Physics",
      "created_at": "2024-01-15T10:30:00"
    }
  ]
}
3. Get Mixed Flashcards
GET /flashcards/mixed

Returns an intelligent mix of flashcards from different subjects.

Query Parameters:

limit (optional): Total number of flashcards to return (1-100, default: 10)
student_id (optional): Filter by specific student
Response:

json
{
  "count": 10,
  "flashcards": [...],
  "subjects_included": ["Physics", "Chemistry", "Biology"]
}
4. Get All Flashcards
GET /flashcards

Retrieves all flashcards.

Query Parameters:

limit (optional): Number of flashcards to return (1-100, default: 10)
student_id (optional): Filter by specific student
5. Get Available Subjects
GET /subjects

Returns all available subjects with their flashcard counts.

Response:

json
{
  "subjects": ["Physics", "Chemistry", "Biology"],
  "subject_counts": {
    "Physics": 5,
    "Chemistry": 3,
    "Biology": 7
  }
}
6. Health Check
GET /health

Check if the API is running.

Response:

json
{
  "status": "healthy",
  "message": "Smart Flashcard API is running"
}
Subject Classification
The system uses an intelligent rule-based approach to classify flashcards into subjects:

Supported Subjects:
Physics: Newton's laws, forces, energy, waves, electricity, etc.
Chemistry: Molecules, reactions, periodic table, acids, bases, etc.
Biology: Cells, DNA, evolution, photosynthesis, ecosystems, etc.
Mathematics: Equations, calculus, algebra, geometry, statistics, etc.
History: Wars, empires, revolutions, historical figures, etc.
Geography: Countries, continents, climate, landforms, etc.
Literature: Novels, poetry, authors, literary devices, etc.
Computer Science: Programming, algorithms, data structures, etc.
General: Default category for unclassified content
Classification Algorithm:
Keyword Matching: Searches for subject-specific keywords in questions and answers
Scoring System: Assigns scores based on keyword frequency and relevance
Confidence Threshold: Ensures reliable classification with confidence checks
Fallback Strategy: Assigns "General" subject when confidence is low
Testing the API
Using curl:
Add a flashcard:
bash
curl -X POST http://localhost:5000/flashcard \
  -H "Content-Type: application/json" \
  -d '{"student_id": "stu001", "question": "What is photosynthesis?", "answer": "The process by which plants convert sunlight into energy"}'
Get flashcards by subject:
bash
curl http://localhost:5000/flashcards/Biology
Get mixed flashcards:
bash
curl http://localhost:5000/flashcards/mixed?limit=5
Using Python requests:
python
import requests

# Add flashcard
response = requests.post('http://localhost:5000/flashcard', json={
    "student_id": "stu001",
    "question": "What is the speed of light?",
    "answer": "299,792,458 meters per second"
})
print(response.json())

# Get flashcards
response = requests.get('http://localhost:5000/flashcards/Physics')
print(response.json())
Database Schema
The application uses SQLite with the following schema:

sql
CREATE TABLE flashcard (
    id INTEGER PRIMARY KEY,
    student_id VARCHAR(50) NOT NULL,
    question TEXT NOT NULL,
    answer TEXT NOT NULL,
    subject VARCHAR(100) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
Error Handling
The API includes comprehensive error handling:

400 Bad Request: Invalid input data, missing fields, or validation errors
404 Not Found: Endpoint not found
405 Method Not Allowed: HTTP method not supported
500 Internal Server Error: Server-side errors
Performance Considerations
Database Indexing: Automatic indexing on primary keys and foreign keys
Query Optimization: Efficient database queries with proper filtering
Memory Management: Reasonable limits on request sizes and response batches
Scalability: Stateless design allows for easy horizontal scaling
Future Enhancements
Machine Learning Integration: Replace rule-based classification with ML models
User Authentication: Add JWT-based authentication
Caching: Implement Redis caching for better performance
Advanced Analytics: Add learning progress tracking
Multiple Languages: Support for non-English flashcards
Contributing
Fork the repository
Create a feature branch
Make your changes
Add tests
Submit a pull request
License
This project is licensed under the MIT License - see the LICENSE file for details.

Contact
For questions or support, please open an issue on GitHub.

