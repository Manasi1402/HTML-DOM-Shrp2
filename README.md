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
        let isEditing = false;
        let editEmail = '';
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
            if (isEditing) {
                updateUserData(email, userDetails);
                isEditing = false;
                editEmail = '';
            } else {
                addUserData(userDetails);
            }
            event.target.reset();
        }
        function addUserData(userDetails) {
            const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
            storedUsers.push(userDetails);
            localStorage.setItem('users', JSON.stringify(storedUsers));
            localStorage.setItem(userDetails.email, JSON.stringify(userDetails));
            showUserOnScreen(userDetails);
        }
        function updateUserData(email, userDetails) {
            const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
            const updatedUsers = storedUsers.map(user => {
                if (user.email === email) {
                    return userDetails;
                }
                return user;
            });
            localStorage.setItem('users', JSON.stringify(updatedUsers));
            localStorage.setItem(userDetails.email, JSON.stringify(userDetails));
            updateUserOnUI(email, userDetails);
        }
        function showUserOnScreen(user) {
            const parentElement = document.getElementById('listofitems');
            const listItem = document.createElement('li');
            listItem.setAttribute('data-email', user.email);
            listItem.textContent = user.name + ' - ' + user.email + ' - ' + user.phonenumber;
            const deleteButton = document.createElement('button');
            deleteButton.textContent = 'Delete';
            deleteButton.addEventListener('click', function() {
                deleteUser(user.email);
            });
            listItem.appendChild(deleteButton);
            parentElement.appendChild(listItem);
        }
        function deleteUser(email) {
            const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
            const updatedUsers = storedUsers.filter(user => user.email !== email);
            localStorage.setItem('users', JSON.stringify(updatedUsers));
            localStorage.removeItem(email);
            removeUserFromUI(email);
        }
        function removeUserFromUI(email) {
            const listItem = document.querySelector(`li[data-email="${email}"]`);
            listItem.remove();
        }
        // Load existing users from local storage and display them on the UI
        window.addEventListener('DOMContentLoaded', function() {
            const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
            storedUsers.forEach(function(user) {
                showUserOnScreen(user);
            });
        });
    </script>
</body>
</html>
