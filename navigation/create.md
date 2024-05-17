---
layout: base 
title: Signup
search_exclude: true
---

<style>
.signup-container {
    display: flex;
    justify-content: space-between;
}

.signup-form {
    width: 45%;
}
</style>

<div class="signup-container">
<!-- Python Signup Form -->
    <form action="javascript:signup_usr()">
        <p>
            <label>
                User ID:
                <input type="text" name="python-uid" id="python-uid" required>
            </label>
        </p>
        <p>
            <label>
                Password:
                <input type="password" name="python-password" id="python-password" required>
            </label>
        </p>
        <p>
            <label>
                Name:
                <input type="text" name="python-fname" id="python-fname" required>
            </label>
        </p>
        <p>
        <!-- dropdown for choosing language (es, fr) -->
            <label>
                Language:
                <select name="python-lang" id="python-lang" required>
                    <option value="es">Spanish</option>
                    <option value="fr">French</option>
                </select>
            </label>
        </p>
        <p>
            <button>Signup</button>
        </p>
    </form>
    <p id="python-message" style="color: white;"></p>
    <script>
        const uri = "http://0.0.0.0:8124/api/users/";
        window.signup_usr = function() {
            var uid = document.getElementById('python-uid').value;
            var password = document.getElementById('python-password').value;
            var name = document.getElementById('python-fname').value;
            var lang = document.getElementById('python-lang').value;
            if (uid == '' || password == '' || name == '' || uid == null || password == null || name == null) {
                return ("Please fill out all fields and ensure the name is at least 2 characters long");
            }
            const options = {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    uid: uid,
                    password: password,
                    name: name,
                    lang: lang
                })
            };
            fetch(uri, options)
                .then(response => response.json())
                .then(data => {
                    if (data.error) {
                        document.getElementById('python-message').innerText = data.error;
                    } else {
                        document.getElementById('python-message').innerText = 'User created';
                    }
                })
                .catch(error => {
                    console.error('There was an error!', error);
                });
        };
    </script>
</div>
