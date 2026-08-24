# Summary of Commands 

## Step 1. 
Access the Server 1 using ssh access with user and hostname:

### ssh tony@stapp01   

## Step 2. 
Verify the working server:

### hostname
/
### whoami

## Step 3. 
Add user with expiration date using -e flag:

### sudo useradd -e 2027-04-15 yousuf

## Step 4.
Verify the user created successfuly or not:

### getent passwd yousuf

## Step 5.
Verify the expiration date of user:

### sudo chage -l yousuf
