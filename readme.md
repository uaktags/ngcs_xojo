# 1&1 NGCS SDK for Xojo
## Created for 1&1 Hackathon - 2017
### By Tim Garrity - Team US Chesterbrook
---
## Installation
To utilize this SDK, you'll need to import the NGCS class into your Xojo Application.

## Usage
Say that you have a Listbox in which you have a few columns:
- Name
- Status
- IP
- Datacenter

Within this listbox, you want all of your servers to be listed for easy viewing within your app.

```VB.net
dim srv as new ngcs.Servers
ngcs.apikey = "<APIKEY>" 'Enter your API key from the CloudPanel =>Management =>Users area
dim resp as new JSONItem(srv.get(str(""), srvPageList.Text.CLong - 1, servPerPageList.text.CLong))
servList.DeleteAllRows
dim i as integer

for i = 0 to resp.Count - 1 'Run through each Server in JsonObject
  dim srvr as JSONItem = resp.Child(i) 'Grab the current Server object
  // NAME
  dim name as string = srvr.value("name") 'Get the "Name" of the Server
  // End NAME
  // STATUS
  dim statusj as jsonitem = srvr.value("status") 'The status is another Json object, we must get the actual "State"
  dim status as string = statusj.Value("state") 'Returns the state of the server
  // END STATUS
  // IP
  dim ip as string 'Create a base string to work with
  dim i2 as integer 'Create a new integer to use as a count
  dim ips as JSONItem = srvr.value("ips") 'Get All the IPs for current server
  dim ipcount as integer= ips.count - 2 'Baseline our count to see if there is more than 1 IP
  ip = ips.child(i2).value("ip") 'Return the first IP listed
  if ipcount > 0 then 'Due to our baseline, we can see that there is either 1 ip (0) or more (>0)
    ip = ip + " +" + str(ipcount) 'Lets just add the number of ips, rather than display them to keep the listbox spacing
  end if
  // END IP
  // DATACENTER
  dim dcj as jsonitem = srvr.value("datacenter") 'Grab the Datacenter jsonobject
  dim dc as string = dcj.value("country_code") 'Find the actual Datacenter countrycode (US, UK, etc)
  // END DATACENTER
  servList.AddRow( _
  name, _
  status, _
  ip, _
  dc _
  )
  servList.RowTag(servList.ListCount-1) = srvr.value("id") 'Lets track the individual row by its ID so we can use it later
next
```

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## History
4/27/17 - Initial Commit

## License
MIT License - See `License.txt`
