# TEST 1 SOLUTION 
High level explaination:
- let assume that our company have a mongodb database which contain about shipping status.
- shipping status instance in database contains 7 fields: 
   + ID: a single string that make shipping post distinct to others.
   + start_location: the coordinates of start location.
   + end_location: the coordinates of end location.
   + current_location: the coordinates of current location
   + receiver_name: name of the receiver.
   + shipping_person: name of shipping person.
   + status: [In Transit, Departed from Facility, Arrived at Facility, Out for Delivery, On Hold]
        > In Transit: The package is on the way to its destination.
        > Departed from Facility: The package has left a sorting or processing facility.
        > Arrived at Facility: The package has arrived at a sorting or processing facility.
        > Out for Delivery: The package is out with the delivery person and should arrive soon.
        > On Hold: The package is temporarily on hold due to an issue, such as incorrect address or customs hold.


- For model, I would use Gemini or OpenAI API instead of hosting my own model.
   + Because using 2 API above give the system not only fast but also high quality response, and hosting a LLM is too expensive as a usable model is 7-20B parameters so hosting my own model is not a optimal solution.
- I would use prompting method like few-shot prompt to manipulate the information extraction from a user query. Therefore, I can use these information to look up for shipping status in our database.

   + For example: prompt = """"System Prompt: You are an assistant that extracts relevant information from a user's query to help locate a specific shipping status from a database. The database contains seven fields for each shipping instance: ID, start_location, end_location, current_location, receiver_name, shipping_person, and status. Your job is to identify the values or parameters needed from the user's question to return an accurate answer."""
   + input: "What's the current status of the shipment with ID 12345XYZ?"
   + output: ShippingStatus.find_one({id: extracted_id})
- Then I would provide output information to LLM and let it create a response based on output information.

