# Dependency Injection
Build complicated apps with ease.

===

## Presentation

===

### What is Dependency Injection?



#### Without Dependency Injection

	class PageWordCounter {
		int Count(string url) {
			var http = new HttpRequest(url);
			return http
				.DownloadString()
				.Split(" ")
				.Count();
		}
	}



#### With Dependency Injection

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}

===

### Why would we want Dependency Injection?

By default developers just spawn what they need

	class PageWordCounter {
		int Count(string url) {
			var http = new HttpRequest(url);
			http.Proxy = SystemConfiguration.DefaultProxy;
			http.Credentials = CredentialsCache.DefaultCredentials;
			return http
				.DownloadString()
				.Split(" ")
				.Count();
		}
	}


It increases the amount of code inside each classes.
- instantiate HttpRequest
- http credentials
- proxy
- proxy credentials
...
(there are potentially a dozens of parameters)


It defocus the class from its unique goal (Counting the words)
- Code about HttpRequest
- Code about Proxy
- Code about credentials


With Dependency Injection, your classes focus
- smaller
- easier to maintain
- testable without their dependency

===

### How do we do it in a real life App
- Manually configure dependencies
- IoC Containers

===

### Full example


## Workshop


### qsdfqsdf


### azeaze


### mlkmlk




# Thank you