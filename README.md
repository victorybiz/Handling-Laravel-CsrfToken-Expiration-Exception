# Handling Laravel CsrfToken Expiration Exception

In Laravel, when you submit a form with expired csrf token, an error with `TokenMismatchException` is thrown.

To handle this exception gracefully and redirect back to the form with an error message and form inputs,
look at your `app/Exceptions/Handler.php`, at the `render($request, Exception $exception)` method/function.

Refactor the `render($request, Exception $exception)` method/function with the codes below.

```php
   public function render($request, Exception $exception)
    {
        
        // ------ Start here
        if ($exception instanceof \Illuminate\Session\TokenMismatchException)
        {
            return redirect()
                    ->back()
                    ->withInput($request->except('password', '_token'))
                    ->with(['alert_danger' => 'Your session timed out. Please try again']);
        }   
        // ------ Ends here


        return parent::render($request, $exception);
    }
```

If the exception thrown is a `TokenMismatchException`, this will handle the exception. 
Ensure to use the `old('field_name')` helper function to repopulate your form and `Session::get('alert_danger')`
to get returned error message.

Happy coding!
