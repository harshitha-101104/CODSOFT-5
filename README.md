# CODSOFT-5
task-5
import tkinter as tk
from tkinter import messagebox
import tkinter.simpledialog as simpledialog

class Contact:
    def __init__(self, name, phone, email, address):
        self.name = name
        self.phone = phone
        self.email = email
        self.address = address

class ContactManagerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Contact Manager")

        self.contacts = []

        self.create_widgets()

    def create_widgets(self):
        # Labels
        tk.Label(self.root, text="Name:").grid(row=0, column=0, sticky="e")
        tk.Label(self.root, text="Phone:").grid(row=1, column=0, sticky="e")
        tk.Label(self.root, text="Email:").grid(row=2, column=0, sticky="e")
        tk.Label(self.root, text="Address:").grid(row=3, column=0, sticky="e")

        # Entry Widgets
        self.name_entry = tk.Entry(self.root)
        self.name_entry.grid(row=0, column=1)

        self.phone_entry = tk.Entry(self.root)
        self.phone_entry.grid(row=1, column=1)

        self.email_entry = tk.Entry(self.root)
        self.email_entry.grid(row=2, column=1)

        self.address_entry = tk.Entry(self.root)
        self.address_entry.grid(row=3, column=1)

        # Buttons
        tk.Button(self.root, text="Add Contact", command=self.add_contact).grid(row=4, column=0, columnspan=2, pady=10)
        tk.Button(self.root, text="View Contacts", command=self.view_contacts).grid(row=5, column=0, columnspan=2, pady=10)
        tk.Button(self.root, text="Search Contact", command=self.search_contact).grid(row=6, column=0, columnspan=2, pady=10)
        tk.Button(self.root, text="Update Contact", command=self.update_contact).grid(row=7, column=0, columnspan=2, pady=10)
        tk.Button(self.root, text="Delete Contact", command=self.delete_contact).grid(row=8, column=0, columnspan=2, pady=10)

    def add_contact(self):
        name = self.name_entry.get()
        phone = self.phone_entry.get()
        email = self.email_entry.get()
        address = self.address_entry.get()

        if name and phone:
            contact = Contact(name, phone, email, address)
            self.contacts.append(contact)
            messagebox.showinfo("Success", "Contact added successfully!")
            self.clear_entries()
        else:
            messagebox.showwarning("Warning", "Name and phone are required fields.")

    def view_contacts(self):
        if not self.contacts:
            messagebox.showinfo("Info", "No contacts available.")
        else:
            contact_list = "\n".join([f"{contact.name}: {contact.phone}" for contact in self.contacts])
            messagebox.showinfo("Contact List", contact_list)

    def search_contact(self):
        search_query = simpledialog.askstring("Search Contact", "Enter name or phone number:")
        if search_query:
            matching_contacts = [contact for contact in self.contacts if
                                 search_query.lower() in contact.name.lower() or
                                 search_query in contact.phone]
            if matching_contacts:
                result = "\n".join([f"{contact.name}: {contact.phone}" for contact in matching_contacts])
                messagebox.showinfo("Search Result", result)
            else:
                messagebox.showinfo("Info", "No matching contacts found.")
        else:
            messagebox.showwarning("Warning", "Please enter a search query.")

    def update_contact(self):
        name_to_update = simpledialog.askstring("Update Contact", "Enter the name to update:")
        if name_to_update:
            contact_to_update = next((contact for contact in self.contacts if contact.name.lower() == name_to_update.lower()), None)
            if contact_to_update:
                updated_phone = simpledialog.askstring("Update Contact", "Enter the updated phone number:")
                if updated_phone:
                    contact_to_update.phone = updated_phone
                    messagebox.showinfo("Success", "Contact updated successfully!")
                else:
                    messagebox.showwarning("Warning", "Please enter a valid phone number.")
            else:
                messagebox.showinfo("Info", "Contact not found.")
        else:
            messagebox.showwarning("Warning", "Please enter a name to update.")

    def delete_contact(self):
        name_to_delete = simpledialog.askstring("Delete Contact", "Enter the name to delete:")
        if name_to_delete:
            contact_to_delete = next((contact for contact in self.contacts if contact.name.lower() == name_to_delete.lower()), None)
            if contact_to_delete:
                self.contacts.remove(contact_to_delete)
                messagebox.showinfo("Success", "Contact deleted successfully!")
            else:
                messagebox.showinfo("Info", "Contact not found.")
        else:
            messagebox.showwarning("Warning", "Please enter a name to delete.")

    def clear_entries(self):
        self.name_entry.delete(0, tk.END)
        self.phone_entry.delete(0, tk.END)
        self.email_entry.delete(0, tk.END)
        self.address_entry.delete(0, tk.END)

if __name__ == "__main__":
    root = tk.Tk()
    app = ContactManagerApp(root)
    root.mainloop()

