### REST (REpresentational Stateless Transfer)
    - A way to provide communcation between systems on the internet.
    - An architectural style which has on six main benefical features:
        - Scalability (the ability to adapt/grow and to interact with large number of services)
        - Generality (generic specification of data transfer, that is independent of technology/programming language)
        - Independence (allows different parts of API to be independent of other)
        - Latency (caching)
        - Security (allows you to support security for example via specific http headers)
        - Encapsulation (allows you to encapsulate the details)
    - HATEOAS (Hypermedia as the engine of the application state) means that when you are desiging an API, you want to pretend that the costumer of the API knows nothing about the API (data format). It's all about representing REST and state in documents and linking those documents to other resources. It's a further restriction on REST architecture.
    - It's not a standard, it's an architecture style and different people interpret it differently. But there are some conventions.
    Resources:
        - Resources are data representations, not behaviour or action. Use nouns.
        - If your resources are "coarse-grained" rather than "fine-grained" (generic rather then specific) you can cover a lot of use-cases.
        - Don't tightly couple resources with behaviour. (You don't want to use it like RPC). Keep it simple.
        - Fundamentally there are two types of resources:
            - Collection resource (collection of ) ex.: /applications
            - Instance resource (single items/docs) ex.: /application/asdas2
        - And we can built robust API's based on these concepts.
    Behavior:
    - HTTP already supports behaviour via it's methods (PUT, GET, POST, DELETE, HEAD). These methods can be used to implement CRUD, it's imporant that it's not a 1:1 correspondence. POST and PUT can be both used to implement both update and create.
    - You can use PUT for create if the client already knows the resource ID, containing all the data for the request.
    - You can use PUT for update for full replacement of existing resource.
    - PUT is an idempontent operation (has no additional effect if it is called more than once with the same input parameters). Put cannot be used for partial update.
    - POST for create is almost always done on a parent resource. (To create a child resource) The response is important in this case (Created status and location)
    - POST as update mostly done on instance resources. You can use it for partial update. This is the only method that is not idempotent. It can be used to reduce to amount of trafic. 
    Media types:
    - Very important, this allows to link to other resources. 
    - Format specification + Parsing rules
    - When you are writing REST client always set accept-header (a coma separated list, in the order of preference), so the server knows how to return data. 
    - The server should always set the content-type header.
    - The nice thing about media types, that you can even create your own. (ex.: application/foo+json so you can apply your custom parsing)
    Design:
    - Instead of thinking in baseurl's, you should be thinking about media types, collections and instances.
    - Make it short (baseurl ex: api.foo.com), treat browsers as clients (for debugging purposes)
    - There are two ways for versioning:
        - embedding the version in the url (not very flexible)
        - using version in the media types (it's very flexible but maybe an overkill)
    - Use UTC and ISO 8061 (ex.: 2007-03-01T13:00:00Z/2008-05-11T15:30:00Z) for representing dates.
    - Using href for resources (so every exposed resource knows it's fully qualified location) can be very important for linking to other documents.
    - In POST methods when it feasable, return the data represntation in the response body. Add override (?_body=false) for control.
    - Hypermedia and Linking is fundamental for scalability. You can grow your system via linkind operations.
    - For example you have a resource that would like to represent a folder (which is not a primitive), in this case you could have a href property which is referencing that object. This can be also applied to collections (not just instance resources).
    - You can specify query parameters for partial representation (to get back only the fields you need) 
    - Pagination can be implemented by offset and limit query parameters, but if you want to follow HATEOAS, you could embed these into the response with references. (link to the first, to the next, to the last)
    - Many to many relationship can be also achieved by using links.
    - Errors should be as descriptive as possible. There are limited number of http error codes for both server and client errror. Custom code property can be very useful for providing as much info as possible.
    - Error 401 means unauthenticated, 403 means unathorized.
    Security:
    - If you can avoid sessions and session ID's. Authenticate every request if necessary, keep them stateless.
    - Do not authenticate based on URL's. Secure based on the content.
    - Use existing security protocols.
    - Use api keys instead of password/username pair. They have much more entropy (not just 12 characters or something). Password reset can mess up things. Don't use sha or md5 for password decryption (collisions, too fast), use bcrypt for exmaple.
    - ID's should be unique globally, don't rely on incremental ID's (at large scale they are very hard to manage). UUID's are okay.
    Caching
    - E-tags can give you a big performance boost.
    Maintance:
    - If you want to move resource from one location to other you can use HTTP redirect.
    - Create abstraction layers when you are migrating.
    - Use well defined media types.
    
### API design:
- When you are developing an API keep in mind you audience (developers). Try to be pragmatic rather then ideologic. Should be easy to adapt. If it's easy to adapt it will scalable. 
- Use two base URLs per resource. 
    ex: /dogs
        /dogs/1234
- Don't use verbs in base url
- Use HTTP verbs to operate on collections and elements.
- Don't mix singular and plural nouns for resources.
- Shouldn't have cases where a URL is deeper than: /resource/identifier/resource
- Example for (dog-owner) association: /owners/5678/dogs
- For complex associations use http question mark: GET /dogs?color=red&state=running&location=park
- Be more specific with errors, return as much info as possible, use the correct status code.
- Never release an API without a version. Make the version mandatory. ex: v1/dogs
- Use partial response (by using optional fields in a comma delimited list) and pagination (limit and offset) to reduce unnecessary data. ex: /joe.smith/friends?fields=id,name,picture
    /dogs?limit=25&offset=50
- For responses that don't return resource it's okay to use verbs (ex.: Algorithms, etc.). /convert?from=EUR&to=CNY&amount=100
- Support multiple formats, and make them specifiable either in the Accept header (true REST) or as a type parameter (?type=json).
- There are basically two models for search:
    - Global search /search?q=fluffy+fur
    - Scoped search /owners/5678/dogs?q=fluffy+fur
