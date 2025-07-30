<!DOCTYPE html>
<html lang="en">
<body>

<h1><code>peakmoment</code>: JWST-MIRI Spectral Analysis and Moment Map Generator</h1>

<h2>Author</h2>
<p>
  This package was developed by <strong>Somnath Dutta</strong><br>
  Academia Sinica Institute of Astronomy and Astrophysics (ASIAA), Taiwan<br>
  Email: <a href="mailto:sdutta@asiaa.sinica.edu.tw">sdutta@asiaa.sinica.edu.tw</a>,
  <a href="mailto:duttasomnath9@gmail.com">duttasomnath9@gmail.com</a>
</p>

<h2>Code Name</h2>
<p>
  <li><code>peakmoment</code></li>
</p>

  
<p>
This code enables the extraction of spectra from JWST image cubes, identification of emission lines, and generation of continuum-subtracted moment-0 maps for each detected spectral peak. 
It uses publicly available <a href="https://archive.stsci.edu/missions-and-data/jwst" target="_blank">JWST archive data</a> to demonstrate the workflow. The data used in this paper were obtained from the Mikulski Archive for Space Telescopes (MAST) at the Space Telescope Science Institute. All the <i>JWST</i> data analyzed here are publicly available in MAST: 
  <a href="https://doi.org/10.17909/52vn-6191" target="_blank">https://doi.org/10.17909/52vn-6191</a>.
</p>


<h3>📓 Jupyter Notebook Demonstration</h3>

<p>
A Jupyter notebook is included to illustrate the workflow for analyzing 
<strong>JWST/MIRI Channel</strong> IFU data. While the example uses Channel 1, 
the same process applies to other MIRI channels with minimal modification.
</p>

<p>
The notebook is structured into two main cells:
</p>

<ul>
  <li><strong>Cell 1:</strong> Extracts the 1D spectrum from a user-defined aperture, 
      identifies spectral peaks above a given significance threshold, and sets up 
      all necessary parameters for moment map generation.</li>
  <li><strong>Cell 2:</strong> Uses the peak information and configuration from Cell 1 to 
      generate <strong>continuum-subtracted moment-0 maps</strong> for each detected emission line.</li>
</ul>

<p>
This notebook provides a convenient, interactive way to explore the data and validate 
the detection and mapping process.
</p>

<h3>🧩 Dependencies</h3>
<p>This script requires the following Python packages:</p>
<ul>
  <li><code>numpy</code></li>
  <li><code>matplotlib</code></li>
  <li><code>spectral_cube</code></li>
  <li><code>astropy</code></li>
  <li><code>scipy</code></li>
  <li><code>tqdm</code></li>
</ul>

<p> Install dependencies with:</p>
<pre><code>pip install numpy matplotlib spectral-cube astropy scipy tqdm</code></pre>

<h3> Create a New Virtual Environment</h3>
 <pre><code>python3 -m venv jwstpeakmoment</code></pre>
  <pre><code>source jwstpeakmoment/bin/activate</code></pre>
  <pre><code>pip install ipykernel</code></pre>
  <pre><code>python -m ipykernel install --user --name=jwstpeakmoment --display-name "jwstpeakmoment"</code></pre>
  <pre><code>jupyter notebook</code></pre>
  <p>In the Notebook, go to <strong>Kernel → Change Kernel</strong> and choose <strong>jwstpeakmoment</strong>. The dependencies should be installed within this virtual environment.</p>


<h2>🔍 JWST/MIRI Spectral Analysis Script</h2>
<p>
The cell #1 script performs aperture-based spectral analysis on <strong>JWST MIRI Channel </strong> (only one channel in this example) IFU data cubes. It extracts a 1D spectrum from a user-defined circular region, fits the continuum, detects emission lines, and annotates known H<sub>2</sub> transitions.
</p>

<h3>✨ Key Features</h3>
<ul>
  <li>📦 Reads a 3D spectral cube from a JWST FITS file using <code>spectral_cube</code>. Spectral axis is converted to microns.</li>
  <li>🔄 Converts the flux units from MJy/sr to mJy/pixel using WCS-derived pixel area in steradians.</li>
  <li>🎯 Applies a circular aperture centered at user-specified pixel (<code>x0</code>, <code>y0</code>) with radius defined in arcseconds  or pixels.</li>
  <li>📉 Extracts the 1D spectrum by summing the flux within the aperture across all wavelengths.</li>
  <li>🧮 Fits a linear continuum to the spectrum using predefined line-free regions.</li>
  <li>🚨 Estimates noise from the continuum and detects peaks above a sigma threshold.</li>
  <li>🧪 Identifies and labels known H<sub>2</sub> lines (e.g., S(5), S(6), S(7)).</li>
  <li>📊 Plots the spectrum with flux, continuum, threshold, and detected lines.</li>
  <li>💾 Saves plots as PNG in <code>moment_maps_per_peak/</code>.</li>
  <li>📋 Prints detected line wavelengths and fluxes to terminal.</li>
</ul>



<h3>📂 Input</h3>
<ul>
  <li>A JWST/MIRI S3D cube in FITS format (e.g., <code>*_s3d.fits</code>).</li>
</ul>

<h3>📤 Output</h3>
<ul>
  <li><strong>PNG plot</strong>: Spectrum plot with annotations and continuum fitting.</li>
  <li>Filename: <code>moment_maps_per_peak/full_spectra__&lt;filename&gt;.png</code></li>
  <li><strong>Terminal output</strong>: Wavelengths and flux values of detected emission peaks.</li>
</ul>

<h3>🛠️ Customization</h3>
<ul>
  <li>Modify <code>x0</code>, <code>y0</code>, and <code>aperture_radius_arcsec</code> or <code>radius</code> (if want to define by number of pixels) for different regions.</li>
  <li>Adjust continuum model degree (default: 1 for linear).</li>
  <li>Change detection threshold <code>sigma_given</code> (default: 5).</li>
</ul>

<hr>

<h2>🌠 JWST MIRI Channel Moment Map Generator (per Emission Line)</h2>
<p>
The cell #2 script builds on spectral analysis results to generate moment-0 maps of detected emission lines. For each detected peak, it fits and subtracts the continuum, integrates over significant channels, and saves diagnostic plots and FITS files.
</p>

<h3>✨ Key Features</h3>
<ul>
  <li>🔄 Processes each spectral peak individually from the previous analysis.</li>
  <li>🎯 Extracts spectra within a defined circular aperture.</li>
  <li>🧮 Fits local continuum using windows bracketing the line.</li>
  <li>📈 Subtracts the continuum to isolate emission.</li>
  <li>🚦 Selects channels with flux > 3σ for moment integration.</li>
  <li>🗺️ Generates four-panel diagnostic plots for each line:
    <ul>
      <li>Spectrum with continuum fit and regions</li>
      <li>Original moment-0 map</li>
      <li>Continuum map</li>
      <li>Continuum-subtracted moment-0 map</li>
    </ul>
  </li>
  <li>💾 Saves:
    <ul>
      <li><code>moment0_original_&lt;wavelength&gt;.fits</code></li>
      <li><code>continuum_map_&lt;wavelength&gt;.fits</code></li>
      <li><code>moment0_subtracted_&lt;wavelength&gt;.fits</code></li>
    </ul>
  </li>
</ul>

<h3>📂 Inputs</h3>
<p><strong>Note:</strong> You do not need to change these parameters if you are using the full version of the notebook. They will be automatically selected based on the output of the previous cell.</p>
<ul>
  <li><code>cube</code>: A <code>SpectralCube</code> object.</li>
  <li><code>detected_peaks_wavelengths</code>: List of emission line wavelengths (μm).</li>
  <li><code>x0, y0</code>: Center of aperture (pixels).</li>
  <li><code>aperture_radius_arcsec</code>: Aperture radius (arcsec) or <code>radius</code>: Aperture radius (pixels).</li>
  <li><code>output_dir</code>: Directory to save output files.</li>
  <li><code>flux_unit</code>: Flux unit (e.g., mJy) for labeling.</li>
</ul>

<h3>📤 Outputs</h3>
<ul>
  <li><strong>PNG plots</strong>:
    <ul>
      <li><code>moment_maps_peak_&lt;index&gt;_&lt;wavelength&gt;.png</code></li>
    </ul>
  </li>
  <li><strong>FITS files</strong> for each peak:
    <ul>
      <li>Original moment-0 map</li>
      <li>Continuum flux map</li>
      <li>Continuum-subtracted moment-0 map</li>
    </ul>
  </li>
  <li><strong>Terminal output</strong>:
    <ul>
      <li>Integrated line flux</li>
      <li>Peak flux (continuum-subtracted)</li>
      <li>Significant channels (above 3σ)</li>
    </ul>
  </li>
</ul>


<h3>🛠️ Customization</h3>
<ul>
  <li><code>delta1</code> and <code>gap1</code> control continuum windows around the line.</li>
  <li>Change 3σ threshold if needed.</li>
  <li>Extend to compute velocity or dispersion maps if desired.</li>
</ul>

<h4>✅ Final Message</h4>
<p>
When complete, the script prints:<br>
<code>Done: CH1 Moment maps generated for all detected peaks.</code>
</p>

<hr>

<h3>📚 Citation</h3>
<p>
If you use this code in your work, please cite the following:
</p>

<blockquote>
Dutta, Somnath. <em>Molecular Jets from an Evolved Protostar: Insights from JWST–ALMA Synergy</em>. Submitted to <strong>ApJ</strong>, 2025. <br>
DOI: <a href="#" target="_blank" rel="noopener noreferrer">Zenodo DOI (to be added upon deposition)</a>
</blockquote>

</body>
</html>
