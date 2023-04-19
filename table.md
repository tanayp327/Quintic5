<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Goals</title>
    <link rel="stylesheet" href="goals.css">
    <link rel="oogabooga">
</head>
<body>
  <main class = "table"> 
      <section class="table_header">
        <h1>Goals</h1>
        <table id = "table">
      <!-- </section> -->
      <section class="table_body">
        <table>
          <thead>
          <tbody id="body">
            <tr>
              <!-- <th> id </th> -->
              <th> Goal </th>
              <th> Difficulty </th>
              <th> Date </th>
            <tr>
            <tbody>
            </tbody>
        <!-- </table> -->


<form action="javascript:createGoal()">
    <p><label>
        Goal:
        <input type="text"  id="goal" required>
    </label></p>
    <p><label>
        Diff:
        <input type="text"  id="diff" required>
    </label></p>
     <p><label>
        Date:
        <input type="text"  id="time" required>
    </label></p>
      <p>
        <button>Create</button>
    </p>
</form>



  <script>

  // prepare HTML result container for new output
  const resultContainer = document.getElementById("body");
  // prepare URL's to allow easy switch from deployment and localhost
  const url = "https://lennsflask.duckdns.org/api/sport"
  // const url = "http://127.0.0.1:8086/api/sport"
  const create_fetch = url + '/create';
  const read_fetch = url + '/';

  // Load users on page entry
  read_goal();


  // Display User Table, data is fetched from Backend Database
  function read_goal() {
    // prepare fetch options
    const read_options = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
      credentials: 'omit', // include, *same-origin, omit
      headers: {
        'Content-Type': 'application/json'
      },
    };

    // fetch the data from API
    fetch(read_fetch, read_options)
      // response is a RESTful "promise" on any successful fetch
      .then(response => {
        // check for response errors
        if (response.status !== 200) {
            const errorMsg = 'Database read error: ' + response.status;
            console.log(errorMsg);
            const tr = document.createElement("tr");
            const td = document.createElement("td");
            td.innerHTML = errorMsg;
            tr.appendChild(td);
            resultContainer.appendChild(tr);
            return;
        }
        // valid response will have json data
        response.json().then(data => {
            console.log(data);
            for (let row in data) {
              console.log(data[row]);
              add_row(data[row]);
            }
        })
    })
    // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
      console.error(err);
      const tr = document.createElement("tr");
      const td = document.createElement("td");
      td.innerHTML = err;
      tr.appendChild(td);
      resultContainer.appendChild(tr);
    });
  }

  function createGoal(){

    console.log(document.getElementById("goal").value)
    const body = {

        goal: document.getElementById("goal").value,
        diff: document.getElementById("diff").value,
        time: document.getElementById("time").value,
    };
    console.log(body)
    const requestOptions = {
        method: 'POST',
        body: JSON.stringify(body),
        headers: {
            "content-type": "application/json",
            'Authorization': 'Bearer my-token',
        },
    };

    // URL for Create API
    // Fetch API call to the database to create a new user
    fetch(create_fetch, requestOptions)
      .then(response => {
        // trap error response from Web API
        if (response.status !== 200) {
          const errorMsg = 'Database create error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
        }
        // response contains valid result
        response.json().then(data => {
            console.log(data);
            //add a table row for the new/created userid
            add_row(body);
        })
    })
  }

  function add_row(data) {
    console.log(data)
    const tr = document.createElement("tr");
    const goal = document.createElement("th");
    const diff = document.createElement("th");
    const time = document.createElement("th");
  

    // obtain data that is specific to the API
    goal.innerHTML = data.goal; 
    diff.innerHTML = data.diff;
    time.innerHTML = data.time; 
    

    // add HTML to container
    tr.appendChild(goal);
    tr.appendChild(diff);
    tr.appendChild(time);

    resultContainer.appendChild(tr);
  }
    </script>


<!-- 
 <form action="http://127.0.0.1:8086/api/sport/create" method="post">
    <label for="goal">Goal:</label><br>
    <input type="text" id="goal" name="goal" placeholder="enter your goal"><br>
    <!--  -->

<!-- 
</form> 
<br>
<br>
<button onclick="addRow()">Add Row</button> --> 




