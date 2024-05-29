---
layout: base 
title: Logout
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
<div class="index">
    <div class="jumbotron jumbotron-fluid" style="text-align: center; ">
        <h2>Thank you for using our service</h2>
        <!-- Prompt user to logout with button -->
        <button id="logoutButton">Logout</button>
        <p>Want To Go Back?</p>
        <a href="javascript:history.back()">Go Back</a>
    </div>
</div>
<script>
document.getElementById('logoutButton').addEventListener('click', function() {
    fetch('http://127.0.0.1:8124/api/users/authenticate', {
        method: 'DELETE',
        credentials: 'include'
    })
    .then(response => {
        if (response.ok) {
            console.log('logout successful');
            //document.cookie = 'jwt=; Max-Age=0; path=/';
            window.location.href = '/derp/';
        } else {
            console.error('failed logout');
        }
    })
    .catch(error => console.error('Error:', error));
});

</script>