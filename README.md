

-------------------------------------------
-------------------------------------------
DOCUMENTATION:
- Default is write to /disk2, read from AWS.
- paddedUrl from AWS is configured to expire in 1 hour (easily changed)
- When should the client directory be created (on AWS and /disk2)?
- Where do I need to call app_set("serializeMem", TRUE) ??

---------------------------
Storage\Engine\Manager
======================

To manage client files the main interactions are with StorageEngineManager.class.php (namespace Storage\Engine\Manager) (and Storage\File\File)





Under the hood There are 2 "Engine" classes: AWSEngine.class.php and Disk2Engine.class.php. Both implement the StorageEngine.interface.php.
---------------------------


Storage\Engine\AWS
Storage\Engine\Disk2
======================
Both these engines implement our Engine interface, which provides the methods below. Each class also contains other methods useful for their own purposes.

createRepo
deleteRepo
deleteFile
getFile
getFileContent
getFileUrl
listFiles
listRepos
putFile
---------------------------

namespace Storage\File
======================
Storage\File\File
-----------------
Properties:
	fileName
	filePath
	size
	eTag (not currently used by Disk2Engine)
	lastModifiedTime
	paddedUrl

Storage\File\Collection (abstract)
Storage\File\(File)Cache 
	extends Collection


Storage\File\Metadata


---------------------------
\Storage\Object
======================
A light-weight abstract class which the other classes extend.

This base class currently does 3 things:

- Adds some convenient help using serialize, __sleep, and __tostring
- Adds (very light) logging functionality which could be extended in future, or just used for dev purposes
- Provides the methods getProperty & setProperty. setProperty returns `$this`, which allows for setter chaining like this:

```php
//{AWSEngine.class.php}->listFiles
	$xenFile = (new \Storage\File\File($this))
				->setProperty('fileName', $listedFileName)
				->setProperty('filePath', $pathData['pathToFile'] . self::PATH_DELIMITER . $listedFileName)
				->setProperty('eTag', $obj['eTag'])
				->setProperty('lastModifiedTime', $obj['LastModified'])
				->setProperty('size', $obj['Size']);
```
