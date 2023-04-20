<html>
<style>
    @import url('https://fonts.googleapis.com/css?family=Poppins:300,400,500,600,700,800,900&display=swap');
    * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
    }
    body {
    min-height: 100vh;
    background: linear-gradient(#2b1055, #7597de);
    }
    header {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    padding: 30px 100px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    }
    header .logo {
    color: #fff;
    font-weight: 700;
    text-decoration: none;
    font-size: 2em;
    text-transform: uppercase;
    letter-spacing: 2px;
    }
    header ul {
    display: flex;
    justify-content: center;
    align-items: center;
    }
    header ul li {
    list-style: none;
    margin-left: 20px;
    }
    header ul li a {
    text-decoration: none;
    padding: 6px 15px;
    color: #fff;
    border-radius: 20px;
    }
    header ul li a:hover,
    header ul li a.active {
    background: #fff;
    color: #2b1055;
    }
</style>
<head>
    <meta charset="UTF-8">
    <title>Exercise API Example</title>
</head>
<body>
    <h1>Exercise API Example</h1>
    <div>
        <h2>Create Exercise</h2>
        <form id="create-form">
            <div>
                <label for="type-input">Type:</label>
                <input type="text" id="type-input" name="type">
            </div>
            <div>
                <label for="duration-input">Duration:</label>
                <input type="number" id="duration-input" name="duration">
            </div>
            <div>
                <label for="reps-input">Reps:</label>
                <input type="number" id="reps-input" name="reps">
            </div>
            <div>
                <label for="calories-input">Calories Burned:</label>
                <input type="number" id="calories-input" name="calories_burned">
            </div>
            <div>
                <label for="date-input">Date:</label>
                <input type="date" id="date-input" name="date">
            </div>
            <div>
                <button type="submit">Create Exercise</button>
            </div>
        </form>
    </div>
    <div>
        <h2>View Exercises</h2>
        <table id="exercise-table">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Type</th>
                    <th>Duration</th>
                    <th>Reps</th>
                    <th>Calories Burned</th>
                    <th>Date</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>
    <script>
        const apiUrl = 'http://127.0.0.1:8086/api/exercises';
        // function to fetch exercises and update the table
        function fetchExercises() {
            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    const tableBody = document.querySelector('#exercise-table tbody');
                    tableBody.innerHTML = '';
                    data.forEach(exercise => {
                        const row = tableBody.insertRow();
                        row.innerHTML = `
                            <td>${exercise.id}</td>
                            <td>${exercise.type}</td>
                            <td>${exercise.duration}</td>
                            <td>${exercise.reps}</td>
                            <td>${exercise.calories_burned}</td>
                            <td>${exercise.date}</td>
                        `;
                    });
                })
                .catch(error => console.error('Error fetching exercises:', error));
        }
        // attach event listener to form submit button
        const createForm = document.querySelector('#create-form');
        createForm.addEventListener('submit', event => {
            event.preventDefault();
            const formData = new FormData(createForm);
            const exerciseData = {};
            formData.forEach((value, key) => exerciseData[key] = value);
            fetch(apiUrl + '/create', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(exerciseData)
            })
            .then(response => response.json())
            .then(data => {
                console.log('Exercise created:', data);
                fetchExercises();
                createForm.reset();
            })
            .catch(error => console.error('Error creating exercise:', error));
        });
        // fetch exercises when page loads
        fetchExercises();
    </script>
</body>
</html>
