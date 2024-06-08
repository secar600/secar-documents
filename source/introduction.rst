 
Introduction
============ 

This manual provides information about SECAR hardware and software, and contains operating procedures for various SECAR subsystems. Certain aspects of the operation of SECAR that are relevant for personnel safety or machine protection are described in various FRIB DCC documents that are referenced where appropriate. 

To clone this respository to be able to have access to the supplement files and instructions, do the following:

.. code-block::
   :caption: How to clone this repository in your home directory
       
        cd ~
        git clone git@github.com:kiana-se/secar-documents.git

.. note::

   To clone the repository, you will need to set up an ssh key and add it to your github account.

To build a pdf file from the material presented in this repository:

- Clone the repository.
- Then do the following:

.. code-block::
   :caption: How to build a pdf file
       
        cd the-path-where-the-repository-is-cloned
        pip3 install -r requirements.txt --user
        make latex
        cd build/latex
        make
