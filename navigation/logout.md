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
    document.cookie = "jwt=; Domain=127.0.0.1; Secure; Path=/; Expires=Thu, 01 Jan 1970 00:00:01 GMT";
    console.log('Logged out');
    window.location.href = "/derp/";
});
</script>