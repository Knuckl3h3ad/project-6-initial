# Dictionary Aggregator Application
This is an example of a microservice application. 

## Aggregator Application
The aggrgator application used for this microservice acts as a front-end for our application. This

It also aggregates and runs various algorithms to return the appropriate data.

The aggregator app has examples of:
- REST services
- REST client 
- 3-tier architecture
- Basic POJO
- Service layer
- Controller layer
- (Spring) dependency injection
- HTTP request objects 
- Logging API 

## Dictionary Application
The dictionary used for this microservice is from https://github.com/matthewreagan/WebstersEnglishDictionary

The dictionary app has examples of:
- REST services
- 3-tier architecture
- Basic POJO
- Static methods
- Static block
- Private methods
- Service layer
- Controller layer
- Data validation
- (Spring) dependency injection
- File I/O using the Java Stream API
- Mapping JSON to an object
- Logging API
- Usage of constants
- Logging performance metrics

## Project 6 (Final) - 200 points

The following are instructions on how to complete project 6 for a grade as well as what you can do for extra credit.

###### Dictionary Application

1. Add a method to the `DictionaryService` that returns all words that end with a certain value.  To do this:
- Copy the `getWordsStartingWith` method
- Change the new method name to `getWordsEndingWith`
- Change the method call inside your new method from .startsWith to .endsWith

2. Add a method to the `DictionaryController` that calls your new service method and returns the words to the calling process.  To do this:
- Copy the `getWordsStartingWith` method
- Change the @GetMapping to `/getWordsEndingWith/{value}`
- Change the new method name to `getWordsEndingWith` 
- Change the method call to the service to `getWordsEndingWith`, which calls the method you created in step 1 above.
- Change the wording of the log message to reflect the new method's functionality.

3. Test your new endpoint using a browser or Postman 

###### Aggregation Application

1. Add a method to the `AggregatorRestClient` that calls your new endpoint from above. To do this:
- Copy the `getWordsStartingWith` method
- Change the new method name to `getWordsEndingWith` 
- Change the URI to the URI of your new dictionary endpoint

2. Add a method to the `AggregatorService` using the code below:
```java
    public List<Entry> getAllPalindromes() {

        final List<Entry> candidates = new ArrayList<>();

        // Iterate from a to z
        IntStream.range('a', '{')
                 .mapToObj(i -> Character.toString(i))
                 .forEach(c -> {

                     // get words starting and ending with character
                     List<Entry> startsWith = restClient.getWordsStartingWith(c);
                     List<Entry> endsWith = restClient.getWordsEndingWith(c);

                     // keep entries that exist in both lists
                     List<Entry> startsAndEndsWith = new ArrayList<>(startsWith);
                     startsAndEndsWith.retainAll(endsWith);

                     // store list with existing entries
                     candidates.addAll(startsAndEndsWith);

                 });

        // test each entry for palindrome, sort and return
        return candidates.stream()
                         .filter(entry -> {
                             String word = entry.getWord();
                             String reverse = new StringBuilder(word).reverse()
                                                                     .toString();
                             return word.equals(reverse);
                         })
                         .sorted()
                         .collect(Collectors.toList());
    }
```
3. Add a method to the `AggregatorController` that consumes the list of **Entries** generated by your new service method created in step 2 above.  To do this:
- Copy the method `getDefinitionFor` 
- Change the `@GetMapping` path of your new method to `/getAllPalindromes`
- Remove the formal parameters from your new method (this endpoint doesn't take any user input).
- On the method header, change the return type to `List<Entry>`
- Change the service call to call the new service method created in step 2 above.

4. Test your new aggregator endpoint using a browser or Postman

##### Extra Credit - 20 points

- For extra credit, rewrite the palindrome service method to NOT use streams.
- You get to decide when/where to use loops, if-statements, etc...
- Method must return the same type (`List<Entry>`)
- You can use an array or whatever you want to generate an array of alpha characters to iterate over.
- Follow the pseudocode in the comments to come up with your own algorithm 
- The ultimate idea is to create an algorithm that gets words that start and end with the same letter and combine them and determine which are palindromes. 
