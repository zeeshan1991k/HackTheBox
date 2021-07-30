# Shell as development user
## Found todo.html in desktop folder of juliette
```powershell
PS C:\Users\juliette\Desktop> ls


    Directory: C:\Users\juliette\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         12/9/2020   6:27 AM            753 todo.html
-ar---         7/29/2021  10:26 PM             34 user.txt


PS C:\Users\juliette\Desktop> type todo.html
<html>
<style>
html{
background:black;
color:orange;
}
table,th,td{
border:1px solid orange;
padding:1em;
border-collapse:collapse;
}
</style>
<table>
        <tr>
            <th>Task</th>
            <th>Status</th>
            <th>Reason</th>
        </tr>
        <tr>
            <td>Configure firewall for port 22 and 445</td>
            <td>Not started</td>
            <td>Unauthorized access might be possible</td>
        </tr>
        <tr>
            <td>Migrate passwords from the Microsoft Store Sticky Notes application to our new password manager</td>
            <td>In progress</td>
            <td>It stores passwords in plain text</td>
        </tr>
        <tr>
            <td>Add new features to password manager</td>
            <td>Not started</td>
            <td>To get promoted, hopefully lol</td>
        </tr>
</table>

</html>
```
which tells that something be interesting in sticky notes application folder.
## Going in Sticky notes application folder
Going in sticky notes application folder `C:\Users\juliette\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState` gives following files.
```powershell
PS C:\Users\juliette\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState> ls


    Directory: C:\Users\juliette\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         1/15/2021   4:10 PM          20480 15cbbc93e90a4d56bf8d9a29305b8981.storage.session
-a----        11/29/2020   3:10 AM           4096 plum.sqlite
-a----         1/15/2021   4:10 PM          32768 plum.sqlite-shm
-a----         1/15/2021   4:10 PM         329632 plum.sqlite-wal


PS C:\Users\juliette\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState>
```
