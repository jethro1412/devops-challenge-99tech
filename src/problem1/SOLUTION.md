Objective: submit a HTTP GET request to https://example.com/api/:order_id with Order IDs that are selling TSLA, writing to output to ./output.txt

**Filter provided data**
- Use jq to filter provided data to find data that selling TSLA.

```
jq -r 'select(.symbol == "TSLA" and .side == "sell")  | .order_id' ./transaction-log.txt 
```
- Output : 
```
12346
12362
```
- Explanation : 
  
  Command jq wih -r option will output the raw data that is provided without any additional quotes, since by default jq will enclose the output in double quotes
  
  jq will filter using parameter 'select(.symbol == "TSLA" and .side == "sell")  | .order_id', this part will filter json objects with  key "symbol" with value "TSLA" and  key "side" with value sell, and after filtering the objects it will extract the value of the order_id key


**Passing the output from jq to curl and output it to a text file**
```
jq -r 'select(.symbol == "TSLA" and .side == "sell") | .order_id' ./transaction-log.txt | xargs -I {} curl -s "https://example.com/api/{} >> output.txt 
```
- Explanation : 
  
  pipe "|" will be used to take output from previous jq command and will pass it to xargs

  "xargs" is a built in linux cmmand that execute command from standard input

  "-I {}" is a parameter that takes the input received from the output of jq command

  "curl" is a command line tool for transferring data with specified URLs
  
  parameter "-s" in curl will make curl operate in silent mode and will not show any progress

  "https://example.com/api/{}" is the target URL with "{}" placeholder will be replaced by order_id from the output of jq command

  ">> ./output.txt" the ">>" operator will append the output of the curl command to a file named output.txt in current working directory

**Full command:**
```
jq -r 'select(.symbol == "TSLA" and .side == "sell") | .order_id' ./transaction-log.txt | xargs -I {} curl -s "https://example.com/api/{} >> output.txt
```