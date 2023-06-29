<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form onsubmit="saveToLocalStorage(event)">
        <label>Name</label>
        <input type="text" name="username" required/>
        <label>EmailId</label>
        <input type="email" name="emailId" required/>
        <label>Phonenumber</label> 
        <input type="tel" name="phonenumber" />
        <button>Submit</button>
    </form>
    <ul id="listofitems"></ul>
    <script>
        function saveToLocalStorage(event) {
            event.preventDefault();
            const name = event.target.username.value;
            const email = event.target.emailId.value;
            const phonenumber = event.target.phonenumber.value;
            const userDetails = {
                name: name,
                email: email,
                phonenumber: phonenumber
            };
            const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
            storedUsers.push(userDetails);
            localStorage.setItem('users', JSON.stringify(storedUsers));
            localStorage.setItem(email, JSON.stringify(userDetails));
            showUserOnScreen(userDetails);
            event.target.reset();
        }
        function showUserOnScreen(user) {
            const parentElement = document.getElementById('listofitems');
            const childElement = document.createElement('li');
            childElement.textContent = user.name + ' - ' + user.email + ' - ' + user.phonenumber;
            parentElement.appendChild(childElement);
        }
     </script>
  </body>
</html>
