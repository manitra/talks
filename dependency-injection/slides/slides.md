# Dependency Injection
Build complicated apps with ease

===

## What is Dependency Injection?


### Without Dependency Injection

	class PageWordCounter {
		int Count(string documentUrl) {
			var http = new HttpRequest();
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}


### With Dependency Injection

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}

===

## Bad things without DI?


### Developers just `new` things

	class PageWordCounter {
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


### Increases the amount of code
- `new HttpRequest`
- credentials
- proxy
- proxy credentials
...
(there are potentially a dozens of parameters)


### Defocuses the class from its job (Counting the words)
- Code about HttpRequest
- Code about Proxy
- Code about credentials

===

## Good things with Dependency Injection


### Focus Fire!
No code about the `http` dependency

It's **ready to use** out of the box

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}


### Easy maintenance
Every class has its job and is easy to maintain
 
 qsdf | qsdf 
 --------
 qsdf|sdf
a|2

	class HttpRequestFactory {
		HttpRequest Create() {
			var result = new HttpRequest();
			// ... configure the request
			return result;
		}
	}

	class HttpRequestFactory {
		HttpRequest Create() {
			var result = new HttpRequest();
			// ... configure the request
			return result;
		}
	}


Configuration

	class HttpRequestFactory {
		HttpRequest Create() {
			var result = new HttpRequest();
			result.Credentials = CredentialsCache.DefaultCredentials;
			result.Proxy = SystemConfig.DefaultProxy;
			result.Timeout = TimeSpan.FromSeconds(120);
			return result;
		}
	}


### Testable without their dependency

===

## How do we do it in a real life App
- Manually configure dependencies
- IoC Containers


### Method injection

	class WordCounter {
		int Count(string documentUrl, HttpRequest http) {
			return http
				.DownloadString(documentUrl)
				.Split(" ")
				.Count();
		}
	}


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

===

## Full example


# Workshop


===

# Thank you
- https://github.com/manitra/talks/tree/master/dependency-injection
- 