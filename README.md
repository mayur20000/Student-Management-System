# Student-Management-System
 a student management system to maintain student records, such as personal information, grades, and attendance. ETC



from flask import Flask, request, jsonify

app = Flask(__name__)

students = [
    {
        "id": 1,
        "name": "John Doe",
        "roll_number": "123456",
        "attendance": [
            {"date": "2022-01-01", "subject": "Mathematics", "status": "Present"},
            {"date": "2022-01-02", "subject": "Physics", "status": "Absent"},
        ],
        "grades": [
            {"subject": "Mathematics", "grade": 85},
            {"subject": "Physics", "grade": 90},
        ],
    },
    {
        "id": 2,
        "name": "Jane Doe",
        "roll_number": "234567",
        "attendance": [
            {"date": "2022-01-01", "subject": "Mathematics", "status": "Present"},
            {"date": "2022-01-02", "subject": "Physics", "status": "Present"},
        ],
        "grades": [
            {"subject": "Mathematics", "grade": 95},
            {"subject": "Physics", "grade": 80},
        ],
    },
]

@app.route("/students", methods=["GET"])
def get_students():
    return jsonify(students)

@app.route("/student/<int:student_id>", methods=["GET"])
def get_student(student_id):
    for student in students:
        if student["id"] == student_id:
            return jsonify(student)
    return jsonify({"message": "Student not found."}), 404

@app.route("/student", methods=["POST"])
def add_student():
    student = {
        "id": students[-1]["id"] + 1,
        "name": request.json["name"],
        "roll_number": request.json["roll_number"],
        "attendance": [],
        "grades": [],
    }
    students.append(student)
    return jsonify(student), 201

@app.route("/student/<int:student_id>", methods=["PUT"])
def update_student(student_id):
    for i, student in enumerate(students):
        if student["id"] == student_id:
            students[i]["name"] = request.json.get("name", student["name"])
            students[i]["roll_number"] = request.json.get("roll_number", student["roll_number"])
            students[i]["attendance"] = request.json.get("attendance", student["attendance"])
            students[i]["grades"] = request.json.get("grades", student["grades"])
            return jsonify(student), 200
    return jsonify({"message": "Student not found."}), 404

@app.route("/student/<int:student_id>", methods=["DELETE"])
def delete_student(student_id):
    for i,
