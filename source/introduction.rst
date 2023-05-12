 
Introduction
============ 

This manual provides information about SECAR hardware and software, and contains standard operating procedures for various SECAR subsystems. I (K. Setoodehnia) do not have the time (and quite frankly the interest) to include the information presented here in FRIB DCC. If the existing DCC documents are in contradiction with this document, it is because they are out-dated, and I did not bother to review, correct and update them. You should trust the information given here over those provided in the DCC documents.

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
        make latex
        cd build/latex
        make
