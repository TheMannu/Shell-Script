
## Cheak and Create User


1. **Prompts for a username** and stores it in the `name` variable.
2. **Adds the user** using the `useradd` command with the `-m` flag, which creates a home directory for the user.
3. **Checks if the user was successfully added** with an `if` statement that tests the exit status (`$?`) of the `useradd` command.
4. **Prompts for a password** using `read -s` to securely capture the password without showing it on the screen.
5. **Sets the password** for the user using `echo "$name:$password" | sudo chpasswd`. The `chpasswd` command updates the user's password.
6. **Handles success or failure** of both adding the user and setting the password, providing feedback accordingly.

If you want to improve it slightly, you could add an additional check for whether the user already exists before proceeding with the `useradd` command, like this:

```bash
#!/bin/bash

# Prompt for the username
echo "Enter your username:"
read name

# Check if the user already exists
if id "$name" &>/dev/null; then
  echo "User $name already exists."
  exit 1
fi

# Adding a new user
sudo useradd -m $name

# Check if the user was added successfully
if [ $? -eq 0 ]; then
  echo "User $name added successfully."
  
  # Prompt for password
  echo "Enter the password for $name:"
  read -s password
  
  # Set the password for the user
  echo "$name:$password" | sudo chpasswd
  
  if [ $? -eq 0 ]; then
    echo "Password set successfully for $name."
  else
    echo "Failed to set the password."
  fi
else
  echo "Failed to add the user."
fi
```

Checks if the user already exists and exits if they do