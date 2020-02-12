## Calibre stack docker setup:

### LazyLibrarian setup

- LazyLibrarian Volume Mapping:
  ```
  - ${USERDIR}/docker/lazylibrarian:/config
  - ${USERDIR}/Downloads/completed:/downloads
  - ${USERDIR}/media/books:/books # same as calibre library
  ```
  
- LazyLibrarian Config: Settings > Processing
  ```
	Download Dir: 
		/downloads/completed/books
	eBook Library Folder: 
		/books
	Calibredb import program: 
		/usr/bin/calibredb # if using linuxserver docker
  ```

### Calibre setup

- Calibre Volume Mapping:
  ```
  - ${USERDIR}/docker/calibre:/config
  - ${USERDIR}/Downloads/completed:/import
  - ${USERDIR}/media/books:/books # same as calibre library
  #- ${USERDIR}/media/librarian:/librarian # optional, just watch folder
  ```
  
- Calibre Config:
  ```
	eBook Naming: <LEAVE AS DEFAULT>
	Calibry Library: /books # if starting new, this should be empty and import from other location
	Automatic adding tab (path): /librarian # optional
  ```

### Calibre-Web setup

- Initial Setup:
* On the initial setup screen, enter /books as your calibre library location
* Unrar is included by default and needs to be set in the Calibre-Web admin page (Basic Configuration:External Binaries) with a path of `/usr/bin/unrar`
* To enable ebook conversion enable docker mod and then in the Calibre-Web admin page (Basic Configuration:External Binaries) set the path to converter tool to `/usr/bin/ebook-convert`

- Calibre-Web Volume Mapping:
  ```
	- ${USERDIR}/media/books:/books
	- ${USERDIR}/docker/calibre_web:/config
  ```
- Calibre-Web Config:
  ```
	Location of Calibre database: /books
  ```
