

.. _Description:

Description
   Determine amplitude calibration from auto-correlations
   
   .. rubric:: Summary
      
   
   .. warning:: **WARNING: accor** is currently an experimental task. Use with
      care and report issues back to the CASA team via the `NRAO
      helpdesk <http://help.nrao.edu>`__. 
   
   **accor** determines the amplitude calibration from
   auto-correlations. 
   
   The **accor** task determines the amplitude corrections from the
   apparent normalization of the mean autocorrelation spectra.
   Mis-normalization of the autocorrelations (and thus also the
   cross-correlations) is caused by errors in sampler thresholds
   during an observation. This correction is typically required for
   data correlated with the DiFX correlator (such as VLBA data).
   Other correlators (such as the SFXC correlator, which is used to
   correlate EVN data at JIVE) may already apply this correction at
   the correlator. In these cases, running this task is not necessary
   (but shouldn't hurt).
   
   The **accor** task should be run with a solution interval
   (*solint*) adequate to track variations in effective sampler level
   optimization (including resets), typically on timescales of
   seconds to minutes.
   
   See `Solving for
   Calibration <https://casa.nrao.edu/casadocs-devel/stable/calibration-and-visibility-data/synthesis-calibration/solving-for-calibration>`__ for
   more information on the task parameters **accor** shares with all
   calibration solving tasks, including data selection, general
   solving properties, and arranging prior calibration
   (i.e., specifying other caltables to pre-apply before solving). In
   most cases, no prior calibration is required, since the raw
   mis-normalization of the autocorrelations is essentially the
   calibration sought from **accor**.
   

.. _Examples:

Examples
   task examples
   
   The following example creates a caltable with **accor** solutions
   on a 30s timescale. 
   
   ::
   
      accor(vis='data.ms', caltable='cal.A', solint='30s')
   

.. _Development:

Development
   task developer
   
   --CASA Developer--
   
   