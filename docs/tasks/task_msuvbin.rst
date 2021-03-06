

.. _Description:

Description
   task description
   
   .. warning:: **WARNING**: This task is currently experimental.
   
   .. rubric:: Summary
      
   
   **msuvbin** is a uv gridding task. It is primarily designed to be
   used for large volumes of data from multiple observing epochs that
   need to be imaged together in order to obtain a final image
   product for the target source or field of interest. One way of
   proceeding with such large multi-epoch data is to image each epoch
   separately and average the images afterward. Instead, **msuvbin**
   averages the visibilities on a common uv grid, then the resulting
   data product can be imaged using the task **clean** or **tclean**.
   Averaging such uv data into a common uv grid first has several
   conveniences and advantages, such as easily doing the proper
   weighted average. More details on this task can be found in the
   `EVLA Memo
   198 <https://library.nrao.edu/public/memos/evla/EVLAM_198.pdf>`__, which
   explains the current implementation in **msuvbin** and its
   limitations. In particular, note the issue/limitation of creating
   a uv grid with wprojection and then using Cotton-Schwab major
   cycles to image it; see the EVLA Memo 198 for more details.
   
    
   
   .. rubric:: Parameter descriptions
      
   
   .. rubric:: *vis*
      
   
   Name of input visibility file
   
   .. rubric:: *field*
      
   
   Field name list; note that this position will define the phase
   center of the output uv grid
   
   .. rubric:: *spw
      *
      
   
   Spectral window selection
   
   .. rubric:: *taql* 
      
   
   TaQl expression for data selection (see  `Data Selection in a
   Measurement
   Set <https://casa.nrao.edu/casadocs-devel/stable/calibration-and-visibility-data/data-selection-in-a-measurementset>`__  or `CASAcore NOTE
   199: Table Query
   Language <https://casacore.github.io/casacore-notes/199.html>`__  for
   more information)
   
   .. rubric:: *outvis*
      
   
   Name of output grid
   
   .. rubric:: *phasecenter*
      
   
   Phase center of the grid, to be used when the phase center of the
   selected field is not the desired output phase center.
   Example: phasecenter='J2000 18h03m04 -20d00m45.1'
   
   .. rubric:: *nx*
      
   
   Number of pixels along the x axis of the grid. Default: 1000
   
   .. rubric:: *ny*
      
   
   Number of pixels along the y axis of the grid. Default: 1000
   
   .. rubric:: *cell*
      
   
   Cellsize of the grid (given in sky units). Default: '1arcsec'
   
   .. rubric:: *ncorr*
      
   
   Number of correlation/polarization plane in uv grid (allowed 1, 2,
   4). For example, if the input data set has the correlations RR and
   LL, and *ncorr* =1, then the output uv grid will be written as
   Stokes I. If *ncorr=* 2, then the output grid will have both the
   RR and LL correlations. Default: 1
   
   .. rubric:: *nchan*
      
   
   Number of spectral channels in the output uv grid. Default: 1
   
   .. rubric:: *fstart*
      
   
   Frequency of the first channel. Default: '1GHz' (the user needs to
   give a useful input here)
   
   .. rubric:: *fstep*
      
   
   Width of spectral channel. Default: '1kHz'
   
   .. rubric:: *wproject*
      
   
   Do wprojection correction while gridding. Default: False
   
   .. rubric:: *memfrac*
      
   
   Controls how much of computer's memory is available for gridding.
   Default=0.5
   

.. _Examples:

Examples
   task examples
   
   Let's assume we have multi-epoch observations on a particular
   field of interest with measurement sets vis_1.ms, vis_2.ms, ...
   vis_n.ms. The task **msuvbin** needs to be executed n times, one
   for each input data set with all the other parameters that define
   the output data set intact. For instance, for the 1st execution of
   **msuvbin**, one may set the following parameters:
   
   ::
   
      | vis = 'vis_1.ms'
      | field = '0'
      | spw = ''
      | taql = ''
      | outvis = "uvgrid.ms'
      | phasecenter = ''
      | nx = 2048
      | ny = 2048
      | cell = '2.0arcsec'
      | ncorr = 2
      | nchan = 320
      | fstart = "1025.00MHz"
      | fstep = "62.5kHz"
      | wproject = False
      | memfrac = 0.9
   
   Here we note the following:
   
   -  Field '0' was selected from the input data, and its position
      will define the phase center of the output uv grid. If another
      position is desired for the phase center, then the parameter
      *phasecenter* needs to be specified.
   
   -  *nx* and *ny* define the number of the pixels along the x and y
      axes of the grid, respectively. The size of each pixel is
      defined by the parameter *cell*. These would be the same values
      that one would use in the task **clean**/**tclean** for
      *imsize* and *cell* to image the output uv grid, and therefore
      need to be set by taking into account the image that one will
      eventually be making.
   
   -  *ncorr* defines the number of the correlations in the output uv
      grid. If the input data set has the correlations RR and LL, and
      *ncorr* is set to 1, then the output uv grid will be written as
      Stokes I. If *ncorr* is set to 2, then the output grid will
      have both the RR and LL correlations.
   
   -  *nchan* determines the number of channels in the output uv grid
      with a frequency width per channel set by the parameter
      *fstep*. The lowest frequency of the output data is set by the
      parameter *fstart*. Note that **msuvbin** will perform on the
      fly Doppler correction; the resulting grid will be in the LSRK
      frame. The *fstart* value is the starting frequency in the LSRK
      frame. The above example will produce a uv grid with 320
      channels starting at 1025 MHz in LSRK, with each channel having
      a width of 62.5 kHz.
   
   -  *memfrac* may be used to set how much memory the task should
      use. In the above example 90% of the available memory will be
      utilized by the task.
   
   After gridding the 1st data set, the task **msuvbin** will need to
   be executed on the other data sets one at a time by changing the
   *vis* parameter only (i.e., *vis='vis_2.ms'*, then
   *vis='vis_3.ms'*, etc...) and keeping the other parameters intact.
   The task **msuvbin** will perform the proper averaging when
   gridding the data sets on the same uv grid. The volume of the
   output data set stays the same regardless of how many measurement
   sets are added onto the same grid.
   

.. _Development:

Development
   task developer
   
   --CASA Developer--
   
   