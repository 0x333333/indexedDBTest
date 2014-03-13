Test for [IndexedDB Polyfill](https://github.com/axemclion/IndexedDBShim.git) [![Build Status](https://secure.travis-ci.org/axemclion/IndexedDBShim.png)](https://travis-ci.org/axemclion/IndexedDBShim)
================================

Test for a polyfill to enable IndexedDB using WebSql.

IndexedDB is not supported on <a href = "http://caniuse.com/#search=IndexedDB" target="_blank">all browsers</a>.
This IndexedDB polyfill exposes the IndexedDB API in unsupported browsers using WebSQL. This shim is basically an IndexedDB-WebSql adapter.

More details of IndexedDB browsers support can be found in this graph:

![pic](http://media-cache-ec0.pinimg.com/originals/d9/7b/b8/d97bb83436eb838b05e9ee213216be00.jpg)

Using this polyfill, you can use a single offline storage API across browsers (Opera, Safari, Firefox, Chrome and IE10) and even mobile devices (Phonegap on iOS and Android).

Using the polyfill
==================
To use the polyfill, simply download [the concatenated, minified product](https://raw.github.com/axemclion/IndexedDBShim/master/dist/IndexedDBShim.min.js) from the `dist` directory and include it in your HTML document.

If IndexedDB is not natively supported (and WebSQL is), the polyfill kicks in and makes a `window.IndexedDB` object available for you to use.
If you want to force the use of this polyfill (on Chrome where both IndexedDB and WebSql are supported), simply use `window.shimIndexedDB.__useShim()`.

#### Initialize database

	if (typeof window.mozIndexedDB !== "undefined") {
	  window.indexedDB = window.mozIndexedDB;
	}
	else {
	  window.indexedDB = window.shimIndexedDB;
	  window.shimIndexedDB.__useShim();     // force to use polyfill
	  window.shimIndexedDB.__debug(true);   // enable debug
	  console.log("Starting Tests with shimIndexedDB");
	}

#### Create database

Normally there are other function that need to add so as to make debug easier, such as `onerror` or `onabort`, etc.

	// Create database
	var dbOpenRequest = window.indexedDB.open('library');

	dbOpenRequest.onsuccess = function(e){
	  console.log("Database Opened successfully");
	  db = dbOpenRequest.result;
	};

	dbOpenRequest.onupgradeneeded = function(e){
	  console.log("Database Upgraded successfully");
	  db = dbOpenRequest.result;
	  // Define data structure and primary key
	  store = db.createObjectStore('books', {
	    keyPath: "isbn",
	    "autoIncrement": false
	  });

	  var isbnIndex = store.createIndex("by_isbn", "isbn", {unique: true});
	  var titleIndex = store.createIndex("by_title", "title");
	  var authorIndex = store.createIndex("by_author", "author");

	  store.put({title: "Quarry Memories", author: "Fred", isbn: 123456});
	  store.put({title: "Water Buffaloes", author: "Fred", isbn: 234567});
	  store.put({title: "Bedrock Nights", author: "Barney", isbn: 345678});

	};

#### Add record

	store.put({title: "Quarry Memories", author: "Fred", isbn: 123456});

#### Add record

	store.delete(id);

#### Clear database

	store.clear();

#### Close connection

	db.close();

Test results
============

Chrome

![pic](http://media-cache-ak0.pinimg.com/originals/6e/f5/09/6ef509f05185950bcad58887cb806dad.jpg)

Firefox

![pic](http://media-cache-ec0.pinimg.com/originals/eb/92/83/eb9283ed6789b74385dd674248971cb9.jpg)

Opera

![pic](http://media-cache-ak0.pinimg.com/736x/78/ad/8f/78ad8f1aeaf0d4f67b84f19af5f92934.jpg)

Safari

![pic](http://media-cache-ak0.pinimg.com/736x/85/c7/2a/85c72ae06cfbb64b1444e44da82d9c28.jpg)