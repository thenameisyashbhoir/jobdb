# jobdb


def connect_database():
    if genderCombo.get() == '' or usernameEntry.get() == '' or passwordEntry.get() == '' or confirmEntry.get() == '' or contactEntry.get() == '' or qualificationCombo.get() == '' or addressEntry.get() == '':
        messagebox.showerror('Error', 'All fields are required')
    elif passwordEntry.get() != confirmEntry.get():
        messagebox.showerror('Error', 'Password Mismatch')
    elif check.get() == 0:
        messagebox.showerror('Error', 'Please accept the terms and conditions')
    else:
        id = idEntry.get()  # Get the ID from the entry field

        con = mysql.connector.connect(host='localhost', user='root', password='YashBhoir@2004', database='userdata')
        mycursor = None
        try:
            mycursor = con.cursor()
            # create table if it doesn't exist
            query = 'CREATE TABLE IF NOT EXISTS login(id int auto_increment primary key not null, email varchar(50), username varchar(100), password varchar(20), contact varchar(20), qualification varchar(50), address varchar(255))'
            mycursor.execute(query)
        except mysql.connector.Error as err:
            if mycursor:
                mycursor.close()
            if con:
                con.close()
            messagebox.showerror('Error', f'{err}')
            return
        try:
            query = 'SELECT * FROM login WHERE username = %s'
            mycursor.execute(query, (usernameEntry.get(),))
            row = mycursor.fetchone()
            if row is not None:
                messagebox.showerror('Error', 'Username Already exists')
            else:
                query = 'INSERT INTO login(id, email, username, password, contact, qualification, address) VALUES(%s, %s, %s, %s, %s, %s, %s)'
                mycursor.execute(query, (id, genderCombo.get(), usernameEntry.get(), passwordEntry.get(), contactEntry.get(), qualificationCombo.get(), addressEntry.get()))
                con.commit()
                messagebox.showinfo('Success', 'Registration is successful')
                clear()
                signup_window.destroy()
                import signin
        except mysql.connector.Error as err:
            messagebox.showerror('Error', f'{err}')
        finally:
            if mycursor:
                mycursor.close()
            if con:
                con.close()


