# Dependency Injection
Build complicated apps with ease

===

## What is Dependency Injection?


### Without Dependency Injection

	class WordCounter {
		int Count(string documentUrl) {
			var http = new HttpRequest();
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}

### With Dependency Injection
```
class WordCounter {
	int Count(string documentUrl, HttpRequest http) {
		return http
			.DownloadString(documentUrl)
			.Split(" ")
			.Count();
	}
}
```

===

## Bad things without DI?


### Developers just `new` things
- new HttpRequest
- credentials
- proxy, proxy credentials

```
class WordCounter {
	int Count(string documentUrl) {
		var http = new HttpRequest();
		http.Proxy = SystemConfiguration.DefaultProxy;
		http.Credentials = CredentialsCache.DefaultCredentials;
		return http
			.DownloadString(documentUrl)
			.Split(" ")
			.Count();
	}
}
```


### Defocuses the class from its job: Counting the words

===

## How Dependency Injection helps about it?


### Focus Fire!
`http` is **ready to use** out of the box

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}



### One class, one job

	void Main(string[] args) {
		var httpFactory = new HttpRequestFactory();
		var counter = new WordCounter();
		var result = counter.Count(args[1], factoryHttp.Create());
		Console.WriteLine(result);
	}

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http.DownloadString(documentUrl).Split(" ").Count();
		}
	}
	class HttpRequestFactory {
		HttpRequest Create() {
			return = new HttpRequest(); // .. + configuration
		}
	}

Note: 
- `Main` is the entry point of the command line tool, its unique responsibility is to combine other class features together with the end user input
- WordCounter: does nothing exept counting
- HttpRequestFactory: instantiate an HttpRequest object with all the parameters like credentials, proxy, keep alive, authentications, cookie ..


### Enhanced testability

	[Test]
	int Count_OnFiveWordsSentence_ReturnsFive() {
		var http = Moq.Of<HttpRequest>(r => 
			r.DownloadString("") == "short sentence of five words"
		);

		var actual = new WordCounter().Count("", http);

		Assert.AreEqual(5, actual);
	}

Note:
- Fast, In Memory, Unitary
- Count is called with a fake http object so that
- We only test the real code of counting words

===

## Multiple levels of injection


### Constructor injection

	class WordCounter {
		private HttpRequest http;

		public WordCounter(HttpRequest http) {
			this.http = http;
		}

		int Count(string documentUrl) {
			return this.http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}

Note:
Considered the best way to inject dependencies because
- Ensure that everyinstance is directly usable after instantiation
- keep method signature small which makes then easier to reuse
- DI Container friendly


### Property injection

	class WordCounter {
		public HttpRequest Http { get; set; }

		int Count(string documentUrl) {
			return Http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}

Note:
The most concise way because there is not boiler plate code in the constructor
Advantages
- keep method signature small which makes then easier to reuse
- DI Container friendly
Disadvantages
- might generate NullRefExceptions instead of meaningful errors
- possibility to create an instance which is not correctly initialized


### Method injection

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}

Note:
All the previous examples were method injection for simplicity.
- Advantages
  - state-less: if your class were thread safe before adding DI, it is also thread safe after
  - Specific: 
- Disadvantages
  - Signatures are ambigious because of a mix of parameters and dependencies
  - Can't be simplified using DI containers

===

## Using DI Containers


### Configuring dependency becomes quickly difficult

	void Main() {
		var http = new HttpRequest();
		var db = new DbConnection();
		var conf = new Configuration(db);
		var counter = new WordCounter(http, config);
		var stats = new StatChecker(counter);

		stats.Check();
	}

Note:
- to use the stats.Check(), one has to know a lot of stuff about the dependencies of that class
- those initialization code could become huge and painful to maintain
- some components have to create objects and sometimes those objects requires dependencies, in that case parents need to depend on children dependency as well


### Containers simplifies this

	interface DiContainer {
		// At one place we register implementations for interfaces
		void Register(interface, implementation);

		//Everywhere else, 
		InterfaceType Resove<InterfaceType>)();
	}
	
Note:
- there is a notion of contracts (usually interfaces) and a notion of implementations (usually a class)
- there are methods to allows developers to register some classes as being implementations of some interfaces
- there are methods to ask implementations for a specific contract
- the container get the corresponding implementation and takes care of filling all its dependencies recursively

===

# Workshop
Have your laptop ready!
- git clone https://github.com/manitra/talks/tree/master/dependency-injection

===

# Thank you
- https://github.com/manitra/talks/tree/master/dependency-injection
- http://www.castleproject.org/projects/windsor/