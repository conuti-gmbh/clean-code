# Code is Hard to Read

When you write code, you have to remember that you are writing code **for yourself and for your colleagues**. 

It should be clean and legible. The computer/compiler doesn't care if the code is well-formed or in one line - it will still be compiled. Keep this in mind every time.

### Before

```php
public function login(string $email, string $user)
{
   …
   if(preg_match("/^([a-z0–9\+_\-]+)(\.[a-z0–9\+_\-]+)*@([a-z0–9\-]+\.)+[a-z]{2,6}$/ix", $email) {
      Auth::login($user);
   }
   …
}
```

### After

```php
public function login(string $email, string $user)
{
   …
   if ($this->isEmailValid()) { 
      Auth::login($user);
   } 
   …
}
private function isEmailValid(string $email) : bool
{
    return (preg_match("/^([a-z0–9\+_\-]+)(\.[a-z0–9\+_\-]+)*@([a-z0–9\-]+\.)+[a-z]{2,6}$/ix", $email));
}
```
