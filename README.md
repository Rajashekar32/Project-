from flask import Flask, jsonify, request

app = Flask(__name__)

# Sample job postings and candidates (for demonstration purposes)
job_postings = [
    {
        "id": 1,
        "title": "Software Engineer",
        "skills": ["JavaScript", "React", "Node.js"],
    },
    {
        "id": 2,
        "title": "Product Manager",
        "skills": ["Product Development", "Market Analysis", "Project Management"],
    },
    {
        "id": 3,
        "title": "Data Scientist",
        "skills": ["Python", "Machine Learning", "Data Analysis"],
    },
]

candidates = [
    {
        "id": 101,
        "name": "John Doe",
        "skills": ["JavaScript", "React", "Node.js", "Java"],
    },
    {
        "id": 102,
        "name": "Jane Smith",
        "skills": ["Product Development", "Project Management", "Marketing"],
    },
    {
        "id": 103,
        "name": "Michael Johnson",
        "skills": ["Python", "Machine Learning", "Data Analysis", "SQL"],
    },
]

@app.route("/job_postings", methods=["GET"])
def get_job_postings():
    return jsonify(job_postings)

@app.route("/candidates", methods=["GET"])
def get_candidates():
    return jsonify(candidates)

@app.route("/match_candidates", methods=["POST"])
def match_candidates():
    job_posting_id = request.json.get("job_posting_id")
    job_posting = next((job for job in job_postings if job["id"] == job_posting_id), None)

    if not job_posting:
        return jsonify({"message": "Job posting not found"}), 404

    matching_candidates = [
        candidate for candidate in candidates if all(skill in candidate["skills"] for skill in job_posting["skills"])
    ]

    return jsonify(matching_candidates)

if __name__ == "__main__":
    app.run(debug=True)
