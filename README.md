

This is the core model described in the article:  
**Covering a Broad Dynamic Range: Information Processing at the Erythropoietin Receptor**   
Verena Becker, Marcel Schilling, Julie Bachmann, Ute Baumann, Andreas Raue,
Thomas Maiwald, Jens Timmer and Ursula Klingmüller; _Science_ Published Online
May 20, 2010; DOI:
[10.1126/science.1184913](http://dx.doi.org/10.1126/science.1184913) PMID:
[20488988](http://www.ncbi.nlm.nih.gov/pubmed/20488988)  
Abstract:  
Cell surface receptors convert extracellular cues into receptor activation,
thereby triggering intracellular signaling networks and controlling cellular
decisions. A major unresolved issue is the identification of receptor
properties that critically determine processing of ligand-encoded information.
We show by mathematical modeling of quantitative data and experimental
validation that rapid ligand depletion and replenishment of cell surface
receptor are characteristic features of the erythropoietin (Epo) receptor
(EpoR). The amount of Epo-EpoR complexes and EpoR activation integrated over
time corresponds linearly to ligand input, covering a broad range of ligand
concentrations. This relation solely depends on EpoR turnover independent of
ligand binding, suggesting an essential role of large intracellular receptor
pools. These receptor properties enable the system to cope with basal and
acute demand in the hematopoietic system.

SBML model exported from PottersWheel.

    
    
    % PottersWheel model definition file
    
    function m = BeckerSchilling2010_EpoR_CoreModel()
    
    m             = pwGetEmptyModel();
    
    %% Meta information
    
    m.ID          = 'BeckerSchilling2010_EpoR_CoreModel';
    m.name        = 'BeckerSchilling2010_EpoR_CoreModel';
    m.description = 'BeckerSchilling2010_EpoR_CoreModel';
    m.authors     = {'Verena Becker',' Marcel Schilling'};
    m.dates       = {'2010'};
    m.type        = 'PW-2-0-42';
    
    %% X: Dynamic variables
    % m = pwAddX(m, ID, startValue, type, minValue, maxValue, unit, compartment, name, description, typeOfStartValue)
    
    m = pwAddX(m, 'EpoR'     ,     516, 'fix'   ,    0, 10000,   [], 'cell', []  , []  , []             , []  , 'protein.generic');
    m = pwAddX(m, 'Epo'      , 2030.19, 'global', 1890,  2310,   [], 'cell', []  , []  , []             , []  , 'protein.generic');
    m = pwAddX(m, 'Epo_EpoR' ,       0, 'fix'   ,    0, 10000,   [], 'cell', []  , []  , []             , []  , 'protein.generic');
    m = pwAddX(m, 'Epo_EpoRi',       0, 'fix'   ,    0, 10000,   [], 'cell', []  , []  , []             , []  , 'protein.generic');
    m = pwAddX(m, 'dEpoi'    ,       0, 'fix'   ,    0, 10000,   [], 'cell', []  , []  , []             , []  , 'protein.generic');
    m = pwAddX(m, 'dEpoe'    ,       0, 'fix'   ,    0, 10000,   [], 'cell', []  , []  , []             , []  , 'protein.generic');
    
    
    %% R: Reactions
    % m = pwAddR(m, reactants, products, modifiers, type, options, rateSignature, parameters, description, ID, name, fast, compartments, parameterTrunks, designerPropsR, stoichiometry, reversible)
    
    m = pwAddR(m, {            }, {'EpoR'      }, {  }, 'C' , [] , 'k1*k2', {'kt','Bmax'}, [], 'reaction0001');
    m = pwAddR(m, {'EpoR'      }, {            }, {  }, 'MA', [] , []     , {'kt'       }, [], 'reaction0002');
    m = pwAddR(m, {'Epo','EpoR'}, {'Epo_EpoR'  }, {  }, 'MA', [] , []     , {'kon'      }, [], 'reaction0003');
    m = pwAddR(m, {'Epo_EpoR'  }, {'Epo','EpoR'}, {  }, 'MA', [] , []     , {'koff'     }, [], 'reaction0004');
    m = pwAddR(m, {'Epo_EpoR'  }, {'Epo_EpoRi' }, {  }, 'MA', [] , []     , {'ke'       }, [], 'reaction0005');
    m = pwAddR(m, {'Epo_EpoRi' }, {'Epo','EpoR'}, {  }, 'MA', [] , []     , {'kex'      }, [], 'reaction0006');
    m = pwAddR(m, {'Epo_EpoRi' }, {'dEpoi'     }, {  }, 'MA', [] , []     , {'kdi'      }, [], 'reaction0007');
    m = pwAddR(m, {'Epo_EpoRi' }, {'dEpoe'     }, {  }, 'MA', [] , []     , {'kde'      }, [], 'reaction0008');
    
    
    
    %% C: Compartments
    % m = pwAddC(m, ID, size,  outside, spatialDimensions, name, unit, constant)
    
    m = pwAddC(m, 'cell', 1);
    
    
    %% K: Dynamical parameters
    % m = pwAddK(m, ID, value, type, minValue, maxValue, unit, name, description)
    
    m = pwAddK(m, 'kt'  , 0.0329366 , 'global', 1e-007, 1000);
    m = pwAddK(m, 'Bmax', 516       , 'fix'   , 492   , 540 );
    m = pwAddK(m, 'kon' , 0.00010496, 'global', 1e-007, 1000);
    m = pwAddK(m, 'koff', 0.0172135 , 'global', 1e-007, 1000);
    m = pwAddK(m, 'ke'  , 0.0748267 , 'global', 1e-007, 1000);
    m = pwAddK(m, 'kex' , 0.00993805, 'global', 1e-007, 1000);
    m = pwAddK(m, 'kdi' , 0.00317871, 'global', 1e-007, 1000);
    m = pwAddK(m, 'kde' , 0.0164042 , 'global', 1e-007, 1000);
    
    
    %% Default sampling time points
    m.t = 0:3:99;
    
    
    %% Y: Observables
    % m = pwAddY(m, rhs, ID, scalingParameter, errorModel, noiseType, unit, name, description, alternativeIDs, designerProps)
    
    m = pwAddY(m, 'Epo + dEpoe'      , 'Epo_extracellular_obs');
    m = pwAddY(m, 'Epo_EpoR'         , 'Epo_cellsurface_obs'  );
    m = pwAddY(m, 'Epo_EpoRi + dEpoi', 'Epo_intracellular_obs');
    
    
    %% S: Scaling parameters
    % m = pwAddS(m, ID, value, type, minValue, maxValue, unit, name, description)
    
    m = pwAddS(m, 'scale_Epo_extracellular_obs', 1, 'fix', 0, 100);
    m = pwAddS(m, 'scale_Epo_cellsurface_obs'  , 1, 'fix', 0, 100);
    m = pwAddS(m, 'scale_Epo_intracellular_obs', 1, 'fix', 0, 100);
    
    
    %% Designer properties (do not modify)
    m.designerPropsM = [1 1 1 0 0 0 400 250 600 400 1 1 1 0 0 0 0];

This model originates from BioModels Database: A Database of Annotated
Published Models. It is copyright (c) 2005-2010 The BioModels.net Team.  
For more information see the [terms of
use](http://www.ebi.ac.uk/biomodels/legal.html) .  
To cite BioModels Database, please use: [Li C, Donizelli M, Rodriguez N,
Dharuri H, Endler L, Chelliah V, Li L, He E, Henry A, Stefan MI, Snoep JL,
Hucka M, Le Novère N, Laibe C (2010) BioModels Database: An enhanced, curated
and annotated resource for published quantitative kinetic models. BMC Syst
Biol., 4:92.](http://www.ncbi.nlm.nih.gov/pubmed/20587024)

