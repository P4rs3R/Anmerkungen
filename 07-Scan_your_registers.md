Security events are stored *by default* in the file:
```
%SystemRoot%\System32\Winevt\Logs\Security.evtx
```
And you can read it usually by `%SystemRoot%\System32\eventvwr.msc`, the [Windows Event viewer](https://en.wikipedia.org/wiki/Event_Viewer).

There's an example of XML code you can use within the Windows Event viewer filter in order to find who did a remote access on your hosts, excluding you:
```
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">*[System[(EventID=4624)]]</Select>
    <Suppress Path="Security">*[System[(EventID=4624)]] and *[EventData[Data[@Name='TargetUserName'] and (Data ='YOUR-HOSTNAME-HERE')]]</Suppress>
    <Suppress Path="Security">*[System[(EventID=4624)]] and *[EventData[Data[@Name='TargetUserName'] and (Data ='YOUR-USERNAME-HERE')]]</Suppress>
  </Query>
</QueryList>
```
where 4624 is the ID for "Access" execution on your PC.
Actually, you can include also your username and your hostname if someone stole your credentials or gained access to your own host.
