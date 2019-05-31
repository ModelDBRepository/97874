The following sequence will populate the NQS database with information from one of
the models in ModelDB.  In the following I downloaded model 44050 from ModelDB
http://senselab.med.yale.edu/senselab/ModelDB/ShowModel.asp?model=44050
'CA1 pyramidal neuron: dendritic spike initiation, Gasparini et al 2004'
This gave me a zip file named modeldb.zip

unzip NQS.zip
unzip MFP.zip
unzip modeldb.zip
cp NQS/vecst.mod modeldb
cd modeldb
nrnivmodl
setenv HOC_LIBRARY_PATH ../NQS:../MFP
    // run special; close native graphics
nrngui forfig5A.hoc 
load_file("setup.hoc")
load_file("mechs.hoc")
trunc(nq,nq2) // create the shape databases
  // At this point you may wish to explore the databases by looking at various NQS commands; eg 
    nq.gets() // show the headers
    nq.pr(10) // show the first 10 entries
    nq2.gets() // show the headers
    nq2.pr(10) // show the first 10 entries
mechnqs(nq2)  // add in the conductance densities
  // Now the conductances have been added to nq2; look again
    nq2.gets() // show the headers
    nq2.pr(10) // show the first 10 entries
nrfp() // will graph out the conductance densities; graph may need to be translated and resized

Note that some models may be far more complicated to load, having several
different subdirectories with mod files, hoc files and templates.  In such cases,
it is necessary to find your way around these directories before using shape.hoc.
In general, it will be necessary to recompile the model with vecst.mod included by
moving vecst.mod to the directory with other mod files and then reexecuting
'nrnivmodl.'  The resulting 'special' can then be run as before with nrngui and
shape.hoc can be loaded with a load_file() command.

Some models may cause failures when loading shape.hoc.  This will occur when
the underlying model to be tested defines something as an object reference or as
a number that is then used differently in shape.hoc.  For example, shape.hoc
defines a procedure p_p().  If an underlying model executed eg 'objref p_p' or
'p_p=7.2' then an error will occur when loading shape.hoc (This problem occurs
because I have not encapsulated shape.hoc in a template so as to protect its
variables.) 
