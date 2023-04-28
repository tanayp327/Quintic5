<head>
    <meta charset="UTF-8">
    <title>Exercise API Example</title>
    <link rel="stylesheet" type="text/css" href="exercise.css">
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
        // function to fetch exercises and update the table
        function fetchExercises() {
            fetch('http://127.0.0.1:8086/api/exercises')
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
            fetch('hhttp://127.0.0.1:8086/api/exercises/create', {
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