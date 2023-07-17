# Comments

It is best to use comments only when you want to explain why something is being done, not what is being done.

The best comment is a good name for a variable, method or class.

### Before

```php
public function login(string $email): array
{
    // getting the User repository
    $ur = $this->entityManager->getRepository(User::class);
    
    // find one user by given email
    $u = $ur->findOneBy([
        'email' => $email,
    ]);
    
    // checking if $user is instance of User
    if (!$u instanceof User) { 
        // throw exception for user not found
        throw new UserNotFoundException('User not found.');
    }
}
```

### After

```php
public function login(string $email): array
{
    $userRepository = $this->entityManager->getRepository(User::class);
    
    $user = $userRepository->findOneBy([
        'email' => $email,
    ]);

    if (!$user instanceof User) { 
        throw new UserNotFoundException('User not found.');
    }
}
```

By using proper names, you don't have to explain each step with a comment.

## Commented sections

Comments are like a deodorant masking the smell of fishy code that could be improved.

### Before

```php
public function printOwing()
{
    $this->printBanner();
    
    // Print details.
    print("name:  " . $this->name);
    print("amount: " . $this->getOutstanding());
}
```

In this case you can just extract the section from that method in a new method and generate cleaner code without useless comments.

### After

```php
public function printOwing() 
{
    $this->printBanner();
    $this->printDetails();
}

private function printDetails() 
{
    print("name:  " . $this->name);
    print("amount: " . $this->getOutstanding());
}
```
