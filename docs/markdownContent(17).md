
![](/images/introduction-validation-layer-0.jpeg)

## What is The Validation Layer

This time I would like to describe how we can protect our REST API applications from requests containing invalid data (data validation process). However, validation of our requests is not enough, unfortunately. In addition to validation, it is our responsibility to return the relevant messages and statuses to our API clients.

> Validation layers are optional components that hook into Vulkan function calls to apply additional operations. Common operations in validation layers are: Checking the values of parameters against the specification to detect misuse. Tracking creation and destruction of objects to find resource leaks.

### Data Validation
What is data validation really? The best definition I found is from [UNECE Data Editing Group](https://statswiki.unece.org/display/kbase/Data+Validation):

> An activity aimed at verifying whether the value of a data item comes from the given (finite or infinite) set of acceptable values. 

According to this definition, we should verify data items that are coming to our application from external sources and check if their values are acceptable. How do we know that the value is acceptable? We need to define data validation rules for every type of data item which is processed in our system.

### Data vs Business Rules validation

I would like to emphasize that data validation is totally different concept than validation of business rules. Data validation is focused on verifying an atomic data item. Business rules validation is a more broad concept and more close to how business works and behaves. So it is mainly focused on behavior. Of course, validating behavior depends on data too, but in a more wide range.

**Examples of data validation:**

- Product order quantity cannot be negative or zero
- Product order quantity should be a number
- Currency of order should be a value from the currencies list

**Examples of business rules validation**

- Product can be ordered only when Customer age is equal to or greater than product minimal age.
- Customer can place only two orders in one day.

### Returning relevant information

If we acknowledge that the rules have been broken during validation, we must stop processing and return the equivalent message to the client. We should follow the following rules:

- we should return messages to the client as fast as possible ([Fail-fast principle](https://en.wikipedia.org/wiki/Fail-fast))
- the reason for the validation error should be well explained and understood by the client
- we should not return technical aspects for security reasons.

### Problem Details for HTTP APIs standard

The issue of returned error messages is so common that a special standard was created describing how to handle such situations. It is called "Problem Details for HTTP APIs standard" and his official description can be found [here](https://datatracker.ietf.org/doc/html/rfc7807). This is an abstract of this standard:

> This document defines a "problem detail" as a way to carry machine-readable details of errors in an HTTP response to avoid the need to define new error response formats for HTTP APIs.

Problem Details standard introduces Problem Details JSON object, which should be part of the response when a validation error occurs. This is a simple canonical model with 5 members:

- problem type
- title
- HTTP status code
- details of the error
- instance (pointer to specific occurrence)

Of course, we can (and sometimes we should) extend this object by adding new properties, but the base should be the same. Thanks to this our API is easier to understand, learn and use. For more detailed information about standards, I invite you to read the documentation which is well described.

### Data validation localization
For the standard application we can put data validation logic in three places:

- GUI – is the entry point for users' input. Data is validated on the client-side, for example using Javascript for web applications
- Application logic/services layer – data is validated in a specific application service or command handler on the server-side
- Database – this is the exit point of request processing and last moment to validate the data

![](/images/introduction-validation-layer-1.png)

In this article, I am omitting GUI and Database components and I am focusing on the server-side of the application. Let’s see how we can implement data validation on the Application Services layer.

We will be implementing the data validation later in the next lessons.


