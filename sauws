123
#!/usr/bin/env python3
import sqlite3
import queue
import sys
import os


############################################################### CONFIG START ###############################################################
createTable = '''CREATE TABLE IF NOT EXISTS CVE(
id INTEGER PRIMARY KEY AUTOINCREMENT,
cveid integer,
name text NOT NULL
);'''


insertToTable = '''INSERT INTO CVE(cveid, name) VALUES(?, ?);
	'''

selectAll = ''' SELECT * FROM CVE;'''



def readSettings():
	return "/root/project/database.db"
############################################################### CONFIG END  ###############################################################


############################################################### DATABASE START ###############################################################
'''
	The Database class implements basic functionality for managaging a sqlite3 database. The class implements functions for
	connecting to a database, disconnecting, creating a new database/table and executing a SQL query
'''
class Database(object):

	path = ""
	conn = None
	queue = None
	c = None

	'''
		Constructor function that initiliazes the super class. Calls function checkSettings to
		find path of default setting file
	'''
	def __init__(self):
		self.checkSettings()
		self.queue = queue.Queue()
	

	'''
		Wrapper function to trigger readSettings. Not fully implemented
	'''
	def checkSettings(self):
		self.path = readSettings()


	'''
		Function to trigger error if the database path is set to ''
	'''
	def checkPath(self):
		if self.path == "":
			#ErrLogger(e)
			print("Path not set!")
			return 0
		else:
			return 1

	'''
		A function that connects to a given database. If the database doesn't exist the function creates it
		and initializes it using a default sqlite3 template
	'''
	def connectToDatabase(self):
		try:
			if(self.checkPath() > 0):
				if( (os.path.isfile(self.path)) is True):
					self.conn = sqlite3.connect(self.path)
				else:
					print("Creating new database file!")
					self.conn = sqlite3.connect(self.path)
					self.createNewDatabase()
			print("Established Connection to database!")
		except Exception as e:
			#ErrLogger(e)
			pass

	'''
		A function that disconnects from a given database, by calling .close(). The function then sets any connection related
		variable to None
	'''
	def disconnectFromDatabase(self):
		if self.conn:
			try:
				self.closeDatabase()
				self.conn = None
				self.c = None
				print("Closed database!")
			except Exception as e:
				#logError(e)
				pass


	'''
		A function that executes the query to create a new table as per the given default
	'''
	def createNewDatabase(self):
			try:
				self.executeQuery(createTable)
				print("Updated new table!")
			except Exception as e:
				#logError(e)
				print(e)


	'''
		A function that executes a query given any number of arguments. The function returns any data recieved
		by the SQL query
	'''
	def executeQuery(self, query, *args):
		print(f"Executing query {query} with arguments {args}")
		if self.conn:
			try:
				self.c = self.conn.cursor()
				data = self.c.execute(query, args[0])
				return data
			except Exception as e:
				#logError(e)
				print(e)
				pass
############################################################### DATABASE END ###############################################################


############################################################### HANDLER START ###############################################################
class DatabaseHandler(Database):

	def putToQueue(self, *args):
		self.queue.put(args)

	def pullFromQueue(self, *args):
		return self.queue.get()


	def emptyQueue(self):
		print("Emptying queue!")
		while(not(self.queue.empty())):
			query = self.pullFromQueue()
			self.insertToDatabase(query[0][0], query[0][1], query[0][2])
			print("Queue empty!")


	def insertToDatabase(self, query, *args):
		self.executeQuery(query, args)
		self.conn.commit()
		pass


	def selectFromDatabase(self, query, *args):
		data = self.executeQuery(query, args)
		if(data == None):
			print("No data found!")
		else:
			for x in data:
				print(x)
############################################################### HANDLER END ###############################################################

#handler = DatabaseHandler()
#handler.connectToDatabase()
#handler.emptyQueue()
#handler.insertToDatabase(insertToTable, 1, "TEST")
#handler.selectFromDatabase(selectAll)


if __name__ == "__main__":
	pass
