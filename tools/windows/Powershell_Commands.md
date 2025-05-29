# Powershell commands

`Get-Help` displays information about a cmdlet. To get help with a particular command, run the following:  
`Get-Help Command-Name`

`Get-Disk` - show the partitioning schemes of the disks on your system.  

`Get-Command` gets all the cmdlets installed on the current Computer. The great thing about this cmdlet is that it allows for pattern matching like the following  
`Get-Command Verb-* or Get-Command *-Noun`

`Select-Object` cmdlet is one way of manipulating objects is pulling out the properties from the output of a cmdlet and creating a new object.
Exp: `Get-ChildItem | Select-Object -Property Mode, Name`

When retrieving output objects, you may want to select objects that match a very specific value. You can do this using the `Where-Object` to filter based on the value of properties. 

The general format for using this cmdlet is 

`Verb-Noun | Where-Object -Property PropertyName -operator Value`

`Verb-Noun | Where-Object {$_.PropertyName -operator Value}`

The second version uses the `$_` operator to iterate through every object passed to the `Where-Object` cmdlet.


Where `-operator` is a list of the following operators:  
    `-Contains`: if any item in the property value is an exact match for the specified value  
    `-EQ`: if the property value is the same as the specified value  
    `-GT`: if the property value is greater than the specified value  

Exp: `Get-Service | Where-Object -Property Status -eq Stopped`

When a cmdlet outputs a lot of information, you may need to sort it to extract the information more efficiently. You do this by pipe-lining the output of a cmdlet to the `Sort-Object` cmdlet  
The format of the command would be:  
`Verb-Noun | Sort-Object`

