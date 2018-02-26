Connect to DGX station
*************************

Use ssh
========

Currently, DGX station can only be accessed via CS network inside Hunter College. You can use ssh via lab computer to access it::
   
    ssh yourname@146.95.214.135

Access DGX station outside::

1. ssh eniac from anywhere using your `eniac account <http://www.geography.hunter.cuny.edu/tbw/CS.Linux.Lab.FAQ/department_of_computer_science.faq.htm>`_::

      ssh yourname@eniac.cs.hunter.cuny.edu

   (Note: Eniac account should be claimed by logging in from one of the Computer Science Linux Labs (1001B or 1001C), or it will be deleted)
2. ssh lab computers from eniac.
3. ssh DGX station(IP: 146.95.214.135) from lab computers::
   
    ssh yourname@146.95.214.135
