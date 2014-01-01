#archive - Dart library to encode and decode various archive and compression formats.
![Build Status](https://drone.io/bitbucket.org/brendan_duncan/archive/status.png)

The library has no reliance on *dart:io*, so it can be used for both server
and web applications.

The archive library currently supports:

**Decoders**:

Zip (Archive) 

Tar (Archive) 

ZLib [Inflate decompression]

GZip [Inflate decompression]

**Encoders**:

Tar (Archive)

*TODO*: Zip, Zlib, GZip

Sample: Extract the contents of a Zip file.  Encode the contents into a Tar file.

    import 'dart:io' as Io;
    import 'package:archive/archive.dart';
    main() {
      // Read the Zip file from disk.
      Io.File file = new Io.File('test.zip');
      file.openSync();
      var bytes = file.readAsBytesSync();
      if (bytes == null) {
        return;
      }
      
      // Decode the Zip file
      var zip = new ZipArchive();
      Archive archive = zip.decode(bytes);
      
      // Extract the contents of the Zip archive to disk.
      for (int i = 0; i < archive.numberOfFiles(); ++i) {
        String filename = archive.fileName(i);
        List<int> data = archive.fileData(i);
        Io.File fp = new Io.File('out/' + filename);
        fp.createSync(recursive: true);
        fp.writeAsBytesSync(data);
      }
      
      // Encode the archive as a Tar file.
      var tar = new TarArchive();
      List<int> tar_data = tar.encode(archive);
      
      // Write the tar file to disk.
      Io.File fp = new Io.File(filename + '.tar');
      fp.createSync(recursive: true);
      fp.writeAsBytesSync(tar_data);
    }