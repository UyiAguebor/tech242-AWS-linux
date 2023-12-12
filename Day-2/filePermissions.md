# File Permissions

## Being the owner of a file does not necessarily mean you have full permissions on that file.
 The permissions for a file are defined by three entities: the owner, the group, and everyone else (referred to as "other"). The owner of a file has the most control over the file's permissions, but they are not the only entity with permissions.

## If you give permissions to the User entity, this means that the user who created the file has the specified permissions.
For example, if you give the User entity read-only permissions, then only the owner of the file can read the file's contents.

## If you give permissions to the Group entity,
this means that all users who are members of the group that owns the file have the specified permissions. For example, if you give the Group entity read and write permissions,then all users who are members of the group can read and modify the file's contents.

## If you give permissions to the Other entity,
this means that all users who are not the owner of the file or a member of the group that owns the file have the specified permissions. For example, if you give the Other entity read, write, and execute permissions, then everyone can read and modify the file's contents and execute the file.

## If you give User permissions are read-only,
Group permissions are read and write, Other permissions are read, write and execute. This means that only the owner of the file can execute the file, but all users can read and modify its contents.

## If you are logged in as the user which is owner of the file,
then you will have read, write, and execute permissions for the file. This is because you are the owner of the file, so you have the most permissions.

## The -rwxr-xr-- permissions in the ls -l output indicate the following:
r: Read permission allows the user to read the file's contents.
w: Write permission allows the user to modify the file's contents.
x: Execute permission allows the user to execute the file (if it is an executable file).
In this case, the owner (user tcboony) has read, write, and execute permissions for the file keeprunning.sh. The group (staff) has read and execute permissions for the file. Everyone else has read permissions for the file.

# File permissions using numeric values

## Numeric values assigned to each permission:

r: Read permission = 4
w: Write permission = 2
x: Execute permission = 1
How to assign read and write permissions:

## To assign read and write permissions to all entities (owner, group, and others), you would give each entity a value of 6. Here's the breakdown:

Owner: 4 + 2 = 6
Group: 4 + 2 = 6
Other: 4 + 2 = 6
This would give all entities read and write permissions.

## Value to assign read, write, and execute permissions:

To assign read, write, and execute permissions to all entities (owner, group, and others), you would give each entity a value of 7. Here's the breakdown:

Owner: 4 + 2 + 1 = 7
Group: 4 + 2 + 1 = 7
Other: 4 + 2 + 1 = 7
This would give all entities read, write, and execute permissions.

## Value to assign read and execute permissions:

To assign read and execute permissions to all entities (owner, group, and others), you would give each entity a value of 5. Here's the breakdown:

Owner: 4 + 1 = 5
Group: 4 + 1 = 5
Other: 4 + 1 = 5
This would give all entities read and execute permissions, but not write permissions.

## What does 644 mean?

The value 644 represents the permissions for a file or directory. It indicates that the owner has read and write permissions, the group has read permissions, and everyone else has read permissions.

The breakdown of the value is as follows:

6: Owner permissions

4: Read permission
2: Write permission
44: Other permissions

4: Read permission
4: Execute permission
Therefore, a file or directory with the permissions 644 indicates that the owner can read and write the file, the group can read the file, and everyone else can read the file.