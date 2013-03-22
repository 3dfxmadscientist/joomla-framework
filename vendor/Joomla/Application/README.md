# The Application Package

## Initialising Applications

`Application\Base` implements an `initialise` method that is called at the end of the constructor. This method is intended to be overriden in derived classes as needed by the developer.

If you are overriding the `__construct` method in your application class, remember to call the parent constructor last.

```php
use Joomla\Application\Base;

class MyApplication extends Base
{
	/**
	 * Customer constructor for my application class.
	 *
	 * @param   Input     $input
	 * @param   Registry  $config  
	 *
	 * @since   1.0
	 */
	public function __construct(Input $input = null, Registry $config = null, Foo $foo)
	{
		// Do some extra assignment.
		$this->foo = $foo;
		
		// Call the parent constructor last of all.
		parent::__construct($input, $config);
	}

	/**
	 * Custom initialisation for my application.
	 *
	 * @return  void
	 *
	 * @since   1.0
	 */
	protected function initialise()
	{
		// Do stuff.
		// Note that configuration has been loaded.
	}
}

```

## Logging within Applications

`Application\Base` implements the `Psr\Log\LoggerAwareInterface` so is ready for intergrating with an logging package that supports that standard.

The following example shows how you could set up logging in your application using `initialise` method from `Application\Base`.

```php
use Joomla\Application\Base;
use Monolog\Monolog;
use Monolog\Handler\NullHandler;
use Monolog\Handler\StreamHandler;

class MyApplication extends Base
{
	/**
	 * Custom initialisation for my application.
	 *
	 * Note that configuration has been loaded.
	 *
	 * @return  void
	 *
	 * @since   1.0
	 */
	protected function initialise()
	{
		// Get the file logging path from configuration.
		$logPath = $this->get('logger.path');
		$log = new Logger('MyApp');
		
		if ($logPath)
		{
			// If the log path is set, configure a file logger.
			$log->pushHandler(new StreamHandler($logPath, Logger::WARNING);
		}
		else
		{
			// If the log path is not set, just use a null logger.
			$log->pushHandler(new NullHandler, Logger::WARNING);
		}
		
		$this->setLogger($logger);
	}
}

```