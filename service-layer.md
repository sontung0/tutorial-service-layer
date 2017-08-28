<h1 align="center">Service Layer</h1>

### Controller

```php
class Controller
{
    protected $service;

    public function __construct()
    {
        $this->service = new ServiceManager();
    }

    public function register()
    {
        return $this->service->user()->register($data);
    }
}
```

### Service Manager

```php
class ServiceManager
{
    /**
     * @var LogServiceInterface
     */
    protected $log;

    /**
     * @var UserServiceInterface
     */
    protected $user;

    public function __construct()
    {
        $this->log = new LogService();
        $this->user = new UserService($this->log);
    }

    public function log()
    {
        return $this->log;
    }

    public function user()
    {
        return $this->user;
    }
}
```

### Log Service

```php
interface LogServiceInterface
{
    public function log($message);
}

class LogService implements LogServiceInterface
{
    public function log($message)
    {
        // log message
    }
}
```

### User Service

```php
interface UserServiceInterface
{
    public function register(array $data);
}

class UserService implements UserServiceInterface
{
    /**
     * @var LogServiceInterface
     */
    protected $logService;

    public function __construct(LogServiceInterface $logService)
    {
        $this->logService = $logService;
    }

    public function register(array $data)
    {
        // DB::insert('users', $data);
        $this->logService->log('Registration successful');
    }
}
...
