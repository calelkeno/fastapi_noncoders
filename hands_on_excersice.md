# Hands-on Excersice : Build a Student Management API using FastAPI (with in-memory storage)

Base on User Management API learned previously, build a simple student management API with CRUD operation using **FastAPI** and store the student data into an in-memory list.

---

## Topic : `Develop an API to Manage Student Records`
**The API should include :**
- Handling Student data (name, age, grade).
- Endpoints should include :
	- GET /students (returns the entire list of users information as JSON.)
	- GET /students/{id} (takes an integer path parameter `id`, searches the list, and returns the matching user) 
	- POST /students (appends the new student information to the in-memory list)
	- PUT /students/{id} (update student data with the provided `id` and data in the request body)
	- DELETE /tasks/{id} (removes the student with the provided `id`)

---

Happy coding! 🚀
Turns natural language prompts into code