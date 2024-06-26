---
layout: base 
title: Login
search_exclude: true
---
<style>
.login-container {
    display: flex;
    justify-content: space-between;
}

.login-form {
    width: 45%;
}
</style>

<div class="login-container">

<!-- Python Login Form -->
<div class="login-form">
    <h1>User Login (Python)</h1>
    <form id="pythonForm" action="javascript:pythonLogin()">
        <p><label>
            User ID:
            <input type="text" name="python-uid" id="python-uid" required>
        </label></p>
        <p><label>
            Password:
            <input type="password" name="python-password" id="python-password" required>
        </label></p>
        <p>
            <button>Login</button>
        </p>
        <p id="python-message" style="color: red;"></p>
    </form>
    <table id="pythonTable">
        <thead>
        <tr>
            <th>Name</th>
            <th>ID</th>
            <th>Age</th>
        </tr>
        </thead>
        <tbody id="pythonResult">
            <!-- javascript generated data -->
        </tbody>
    </table>
</div>



<script type="module">
    import { login, javaURI, pythonURI, fetchOptions } from '/derp/assets/js/api/config.js';

    // Method to login user
    window.pythonLogin = function() {
        // Set login options
        const options = {};
        // Authentication endpoint
        options.URL = 'http://127.0.0.1:8124/api/users/authenticate';
        options.callback = pythonDatabase;  // method to call on success
        options.message = "python-message"; 
        // Set fetch options
        options.method = "POST";
        options.cache = "no-cache";
        options.body = {
            uid: document.getElementById("python-uid").value,
            password: document.getElementById("python-password").value,
        };
        login(options);
        window.href = '/derp/navigation/login.html';
    }

    function pythonDatabase() {
       const URL = pythonURI + '/api/users/';
       // Define the loginForm and dataTable variables
       const loginForm = document.getElementById('pythonForm');
       const dataTable = document.getElementById('pythonTable');

        // prepare HTML result container for new output
        const resultContainer = document.getElementById("pythonResult");
        resultContainer.innerHTML = ''; // clear each access

        // fetch the API
        fetch(URL, fetchOptions)
            // response is a RESTful "promise" on any successful fetch
            .then(response => {
            // check for response errors and display
            if (response.status !== 200) {
                // fails, show login form and hide data
                loginForm.style.display = 'block';
                dataTable.style.display = 'none';

                const errorMsg = "Flask server response: " + response.status;
                console.log(errorMsg);
                const tr = document.createElement("tr");
                const td = document.createElement("td");
                td.innerHTML = errorMsg;
                tr.appendChild(td);
                resultContainer.appendChild(tr);
                return;
            }
            // valid response will contain JSON data
            loginForm.style.display = 'none';
            dataTable.style.display = 'block';

            response.json().then(data => {
                console.log(data);
                for (const row of data) {
                    // tr and td build out for each row
                    const tr = document.createElement("tr");
                    const name = document.createElement("td");
                    const id = document.createElement("td");
                    const age = document.createElement("td");
                    // data is specific to the API
                    name.innerHTML = row.name; 
                    id.innerHTML = row.uid; 
                    age.innerHTML = row.age; 
                    // this builds td's into tr
                    tr.appendChild(name);
                    tr.appendChild(id);
                    tr.appendChild(age);
                    // append the row to table
                    resultContainer.appendChild(tr);
                }
            })
        })
        // catch fetch errors (ie ACCESS to server blocked)
        .catch(err => {
           // fails, show login form and hide data
            loginForm.style.display = 'block';
            dataTable.style.display = 'none'; 

            console.error("Network error: " + err);
            const tr = document.createElement("tr");
            const td = document.createElement("td");
            td.innerHTML = err + ": " + URL;
            tr.appendChild(td);
            resultContainer.appendChild(tr);
        });
    }

    window.onload = function() {
        pythonDatabase();
    };
</script>
