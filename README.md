# arupduttabappy.github.io
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a><img src=\"https://ibm.box.com/shared/static/ugcqz6ohbvff804xp84y4kqnvvk3bq1g.png\" width=\"200\" align=\"center\"></a>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h1>Analyzing US Economic Data and  Building a Dashboard  </h1>\n",
    "<h2>Description</h2>\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Extracting essential data from a dataset and displaying it is a necessary part of data science; therefore individuals can make correct decisions based on the data. In this assignment, you will extract some essential economic indicators from some data, you will then display these economic indicators in a Dashboard. You can then share the dashboard via an URL.\n",
    "<p>\n",
    "<a href=\"https://en.wikipedia.org/wiki/Gross_domestic_product\"> Gross domestic product (GDP)</a> is a measure of the market value of all the final goods and services produced in a period. GDP is an indicator of how well the economy is doing. A drop in GDP indicates the economy is producing less; similarly an increase in GDP suggests the economy is performing better. In this lab, you will examine how changes in GDP impact the unemployment rate. You will take screen shots of every step, you will share the notebook and the URL pointing to the dashboard.</p>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2>Table of Contents</h2>\n",
    "<div class=\"alert alert-block alert-info\" style=\"margin-top: 20px\">\n",
    "    <ul>\n",
    "        <li><a href=\"#Section_1\"> Define a Function that Makes a Dashboard </a></li>\n",
    "    <li><a href=\"#Section_2\">Question 1: Create a dataframe that contains the GDP data and display it</a> </li>\n",
    "    <li><a href=\"#Section_3\">Question 2: Create a dataframe that contains the unemployment data and display it</a></li>\n",
    "    <li><a href=\"#Section_4\">Question 3: Display a dataframe where unemployment was greater than 8.5%</a></li>\n",
    "    <li><a href=\"#Section_5\">Question 4: Use the function make_dashboard to make a dashboard</a></li>\n",
    "        <li><a href=\"#Section_6\"><b>(Optional not marked)</b> Save the dashboard on IBM cloud and display it</a></li>\n",
    "    </ul>\n",
    "<p>\n",
    "    Estimated Time Needed: <strong>180 min</strong></p>\n",
    "</div>\n",
    "\n",
    "<hr>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2 id=\"Section_1\"> Define Function that Makes a Dashboard  </h2>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We will import the following libraries."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "\n",
       "    <div class=\"bk-root\">\n",
       "        <a href=\"https://bokeh.org\" target=\"_blank\" class=\"bk-logo bk-logo-small bk-logo-notebook\"></a>\n",
       "        <span id=\"1001\">Loading BokehJS ...</span>\n",
       "    </div>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "application/javascript": [
       "\n",
       "(function(root) {\n",
       "  function now() {\n",
       "    return new Date();\n",
       "  }\n",
       "\n",
       "  var force = true;\n",
       "\n",
       "  if (typeof root._bokeh_onload_callbacks === \"undefined\" || force === true) {\n",
       "    root._bokeh_onload_callbacks = [];\n",
       "    root._bokeh_is_loading = undefined;\n",
       "  }\n",
       "\n",
       "  var JS_MIME_TYPE = 'application/javascript';\n",
       "  var HTML_MIME_TYPE = 'text/html';\n",
       "  var EXEC_MIME_TYPE = 'application/vnd.bokehjs_exec.v0+json';\n",
       "  var CLASS_NAME = 'output_bokeh rendered_html';\n",
       "\n",
       "  /**\n",
       "   * Render data to the DOM node\n",
       "   */\n",
       "  function render(props, node) {\n",
       "    var script = document.createElement(\"script\");\n",
       "    node.appendChild(script);\n",
       "  }\n",
       "\n",
       "  /**\n",
       "   * Handle when an output is cleared or removed\n",
       "   */\n",
       "  function handleClearOutput(event, handle) {\n",
       "    var cell = handle.cell;\n",
       "\n",
       "    var id = cell.output_area._bokeh_element_id;\n",
       "    var server_id = cell.output_area._bokeh_server_id;\n",
       "    // Clean up Bokeh references\n",
       "    if (id != null && id in Bokeh.index) {\n",
       "      Bokeh.index[id].model.document.clear();\n",
       "      delete Bokeh.index[id];\n",
       "    }\n",
       "\n",
       "    if (server_id !== undefined) {\n",
       "      // Clean up Bokeh references\n",
       "      var cmd = \"from bokeh.io.state import curstate; print(curstate().uuid_to_server['\" + server_id + \"'].get_sessions()[0].document.roots[0]._id)\";\n",
       "      cell.notebook.kernel.execute(cmd, {\n",
       "        iopub: {\n",
       "          output: function(msg) {\n",
       "            var id = msg.content.text.trim();\n",
       "            if (id in Bokeh.index) {\n",
       "              Bokeh.index[id].model.document.clear();\n",
       "              delete Bokeh.index[id];\n",
       "            }\n",
       "          }\n",
       "        }\n",
       "      });\n",
       "      // Destroy server and session\n",
       "      var cmd = \"import bokeh.io.notebook as ion; ion.destroy_server('\" + server_id + \"')\";\n",
       "      cell.notebook.kernel.execute(cmd);\n",
       "    }\n",
       "  }\n",
       "\n",
       "  /**\n",
       "   * Handle when a new output is added\n",
       "   */\n",
       "  function handleAddOutput(event, handle) {\n",
       "    var output_area = handle.output_area;\n",
       "    var output = handle.output;\n",
       "\n",
       "    // limit handleAddOutput to display_data with EXEC_MIME_TYPE content only\n",
       "    if ((output.output_type != \"display_data\") || (!output.data.hasOwnProperty(EXEC_MIME_TYPE))) {\n",
       "      return\n",
       "    }\n",
       "\n",
       "    var toinsert = output_area.element.find(\".\" + CLASS_NAME.split(' ')[0]);\n",
       "\n",
       "    if (output.metadata[EXEC_MIME_TYPE][\"id\"] !== undefined) {\n",
       "      toinsert[toinsert.length - 1].firstChild.textContent = output.data[JS_MIME_TYPE];\n",
       "      // store reference to embed id on output_area\n",
       "      output_area._bokeh_element_id = output.metadata[EXEC_MIME_TYPE][\"id\"];\n",
       "    }\n",
       "    if (output.metadata[EXEC_MIME_TYPE][\"server_id\"] !== undefined) {\n",
       "      var bk_div = document.createElement(\"div\");\n",
       "      bk_div.innerHTML = output.data[HTML_MIME_TYPE];\n",
       "      var script_attrs = bk_div.children[0].attributes;\n",
       "      for (var i = 0; i < script_attrs.length; i++) {\n",
       "        toinsert[toinsert.length - 1].firstChild.setAttribute(script_attrs[i].name, script_attrs[i].value);\n",
       "      }\n",
       "      // store reference to server id on output_area\n",
       "      output_area._bokeh_server_id = output.metadata[EXEC_MIME_TYPE][\"server_id\"];\n",
       "    }\n",
       "  }\n",
       "\n",
       "  function register_renderer(events, OutputArea) {\n",
       "\n",
       "    function append_mime(data, metadata, element) {\n",
       "      // create a DOM node to render to\n",
       "      var toinsert = this.create_output_subarea(\n",
       "        metadata,\n",
       "        CLASS_NAME,\n",
       "        EXEC_MIME_TYPE\n",
       "      );\n",
       "      this.keyboard_manager.register_events(toinsert);\n",
       "      // Render to node\n",
       "      var props = {data: data, metadata: metadata[EXEC_MIME_TYPE]};\n",
       "      render(props, toinsert[toinsert.length - 1]);\n",
       "      element.append(toinsert);\n",
       "      return toinsert\n",
       "    }\n",
       "\n",
       "    /* Handle when an output is cleared or removed */\n",
       "    events.on('clear_output.CodeCell', handleClearOutput);\n",
       "    events.on('delete.Cell', handleClearOutput);\n",
       "\n",
       "    /* Handle when a new output is added */\n",
       "    events.on('output_added.OutputArea', handleAddOutput);\n",
       "\n",
       "    /**\n",
       "     * Register the mime type and append_mime function with output_area\n",
       "     */\n",
       "    OutputArea.prototype.register_mime_type(EXEC_MIME_TYPE, append_mime, {\n",
       "      /* Is output safe? */\n",
       "      safe: true,\n",
       "      /* Index of renderer in `output_area.display_order` */\n",
       "      index: 0\n",
       "    });\n",
       "  }\n",
       "\n",
       "  // register the mime type if in Jupyter Notebook environment and previously unregistered\n",
       "  if (root.Jupyter !== undefined) {\n",
       "    var events = require('base/js/events');\n",
       "    var OutputArea = require('notebook/js/outputarea').OutputArea;\n",
       "\n",
       "    if (OutputArea.prototype.mime_types().indexOf(EXEC_MIME_TYPE) == -1) {\n",
       "      register_renderer(events, OutputArea);\n",
       "    }\n",
       "  }\n",
       "\n",
       "  \n",
       "  if (typeof (root._bokeh_timeout) === \"undefined\" || force === true) {\n",
       "    root._bokeh_timeout = Date.now() + 5000;\n",
       "    root._bokeh_failed_load = false;\n",
       "  }\n",
       "\n",
       "  var NB_LOAD_WARNING = {'data': {'text/html':\n",
       "     \"<div style='background-color: #fdd'>\\n\"+\n",
       "     \"<p>\\n\"+\n",
       "     \"BokehJS does not appear to have successfully loaded. If loading BokehJS from CDN, this \\n\"+\n",
       "     \"may be due to a slow or bad network connection. Possible fixes:\\n\"+\n",
       "     \"</p>\\n\"+\n",
       "     \"<ul>\\n\"+\n",
       "     \"<li>re-rerun `output_notebook()` to attempt to load from CDN again, or</li>\\n\"+\n",
       "     \"<li>use INLINE resources instead, as so:</li>\\n\"+\n",
       "     \"</ul>\\n\"+\n",
       "     \"<code>\\n\"+\n",
       "     \"from bokeh.resources import INLINE\\n\"+\n",
       "     \"output_notebook(resources=INLINE)\\n\"+\n",
       "     \"</code>\\n\"+\n",
       "     \"</div>\"}};\n",
       "\n",
       "  function display_loaded() {\n",
       "    var el = document.getElementById(\"1001\");\n",
       "    if (el != null) {\n",
       "      el.textContent = \"BokehJS is loading...\";\n",
       "    }\n",
       "    if (root.Bokeh !== undefined) {\n",
       "      if (el != null) {\n",
       "        el.textContent = \"BokehJS \" + root.Bokeh.version + \" successfully loaded.\";\n",
       "      }\n",
       "    } else if (Date.now() < root._bokeh_timeout) {\n",
       "      setTimeout(display_loaded, 100)\n",
       "    }\n",
       "  }\n",
       "\n",
       "\n",
       "  function run_callbacks() {\n",
       "    try {\n",
       "      root._bokeh_onload_callbacks.forEach(function(callback) {\n",
       "        if (callback != null)\n",
       "          callback();\n",
       "      });\n",
       "    } finally {\n",
       "      delete root._bokeh_onload_callbacks\n",
       "    }\n",
       "    console.debug(\"Bokeh: all callbacks have finished\");\n",
       "  }\n",
       "\n",
       "  function load_libs(css_urls, js_urls, callback) {\n",
       "    if (css_urls == null) css_urls = [];\n",
       "    if (js_urls == null) js_urls = [];\n",
       "\n",
       "    root._bokeh_onload_callbacks.push(callback);\n",
       "    if (root._bokeh_is_loading > 0) {\n",
       "      console.debug(\"Bokeh: BokehJS is being loaded, scheduling callback at\", now());\n",
       "      return null;\n",
       "    }\n",
       "    if (js_urls == null || js_urls.length === 0) {\n",
       "      run_callbacks();\n",
       "      return null;\n",
       "    }\n",
       "    console.debug(\"Bokeh: BokehJS not loaded, scheduling load and callback at\", now());\n",
       "    root._bokeh_is_loading = css_urls.length + js_urls.length;\n",
       "\n",
       "    function on_load() {\n",
       "      root._bokeh_is_loading--;\n",
       "      if (root._bokeh_is_loading === 0) {\n",
       "        console.debug(\"Bokeh: all BokehJS libraries/stylesheets loaded\");\n",
       "        run_callbacks()\n",
       "      }\n",
       "    }\n",
       "\n",
       "    function on_error() {\n",
       "      console.error(\"failed to load \" + url);\n",
       "    }\n",
       "\n",
       "    for (var i = 0; i < css_urls.length; i++) {\n",
       "      var url = css_urls[i];\n",
       "      const element = document.createElement(\"link\");\n",
       "      element.onload = on_load;\n",
       "      element.onerror = on_error;\n",
       "      element.rel = \"stylesheet\";\n",
       "      element.type = \"text/css\";\n",
       "      element.href = url;\n",
       "      console.debug(\"Bokeh: injecting link tag for BokehJS stylesheet: \", url);\n",
       "      document.body.appendChild(element);\n",
       "    }\n",
       "\n",
       "    for (var i = 0; i < js_urls.length; i++) {\n",
       "      var url = js_urls[i];\n",
       "      var element = document.createElement('script');\n",
       "      element.onload = on_load;\n",
       "      element.onerror = on_error;\n",
       "      element.async = false;\n",
       "      element.src = url;\n",
       "      console.debug(\"Bokeh: injecting script tag for BokehJS library: \", url);\n",
       "      document.head.appendChild(element);\n",
       "    }\n",
       "  };var element = document.getElementById(\"1001\");\n",
       "  if (element == null) {\n",
       "    console.error(\"Bokeh: ERROR: autoload.js configured with elementid '1001' but no matching script tag was found. \")\n",
       "    return false;\n",
       "  }\n",
       "\n",
       "  function inject_raw_css(css) {\n",
       "    const element = document.createElement(\"style\");\n",
       "    element.appendChild(document.createTextNode(css));\n",
       "    document.body.appendChild(element);\n",
       "  }\n",
       "\n",
       "  \n",
       "  var js_urls = [\"https://cdn.pydata.org/bokeh/release/bokeh-1.4.0.min.js\", \"https://cdn.pydata.org/bokeh/release/bokeh-widgets-1.4.0.min.js\", \"https://cdn.pydata.org/bokeh/release/bokeh-tables-1.4.0.min.js\", \"https://cdn.pydata.org/bokeh/release/bokeh-gl-1.4.0.min.js\"];\n",
       "  var css_urls = [];\n",
       "  \n",
       "\n",
       "  var inline_js = [\n",
       "    function(Bokeh) {\n",
       "      Bokeh.set_log_level(\"info\");\n",
       "    },\n",
       "    function(Bokeh) {\n",
       "    \n",
       "    \n",
       "    }\n",
       "  ];\n",
       "\n",
       "  function run_inline_js() {\n",
       "    \n",
       "    if (root.Bokeh !== undefined || force === true) {\n",
       "      \n",
       "    for (var i = 0; i < inline_js.length; i++) {\n",
       "      inline_js[i].call(root, root.Bokeh);\n",
       "    }\n",
       "    if (force === true) {\n",
       "        display_loaded();\n",
       "      }} else if (Date.now() < root._bokeh_timeout) {\n",
       "      setTimeout(run_inline_js, 100);\n",
       "    } else if (!root._bokeh_failed_load) {\n",
       "      console.log(\"Bokeh: BokehJS failed to load within specified timeout.\");\n",
       "      root._bokeh_failed_load = true;\n",
       "    } else if (force !== true) {\n",
       "      var cell = $(document.getElementById(\"1001\")).parents('.cell').data().cell;\n",
       "      cell.output_area.append_execute_result(NB_LOAD_WARNING)\n",
       "    }\n",
       "\n",
       "  }\n",
       "\n",
       "  if (root._bokeh_is_loading === 0) {\n",
       "    console.debug(\"Bokeh: BokehJS loaded, going straight to plotting\");\n",
       "    run_inline_js();\n",
       "  } else {\n",
       "    load_libs(css_urls, js_urls, function() {\n",
       "      console.debug(\"Bokeh: BokehJS plotting callback run at\", now());\n",
       "      run_inline_js();\n",
       "    });\n",
       "  }\n",
       "}(window));"
      ],
      "application/vnd.bokehjs_load.v0+json": "\n(function(root) {\n  function now() {\n    return new Date();\n  }\n\n  var force = true;\n\n  if (typeof root._bokeh_onload_callbacks === \"undefined\" || force === true) {\n    root._bokeh_onload_callbacks = [];\n    root._bokeh_is_loading = undefined;\n  }\n\n  \n\n  \n  if (typeof (root._bokeh_timeout) === \"undefined\" || force === true) {\n    root._bokeh_timeout = Date.now() + 5000;\n    root._bokeh_failed_load = false;\n  }\n\n  var NB_LOAD_WARNING = {'data': {'text/html':\n     \"<div style='background-color: #fdd'>\\n\"+\n     \"<p>\\n\"+\n     \"BokehJS does not appear to have successfully loaded. If loading BokehJS from CDN, this \\n\"+\n     \"may be due to a slow or bad network connection. Possible fixes:\\n\"+\n     \"</p>\\n\"+\n     \"<ul>\\n\"+\n     \"<li>re-rerun `output_notebook()` to attempt to load from CDN again, or</li>\\n\"+\n     \"<li>use INLINE resources instead, as so:</li>\\n\"+\n     \"</ul>\\n\"+\n     \"<code>\\n\"+\n     \"from bokeh.resources import INLINE\\n\"+\n     \"output_notebook(resources=INLINE)\\n\"+\n     \"</code>\\n\"+\n     \"</div>\"}};\n\n  function display_loaded() {\n    var el = document.getElementById(\"1001\");\n    if (el != null) {\n      el.textContent = \"BokehJS is loading...\";\n    }\n    if (root.Bokeh !== undefined) {\n      if (el != null) {\n        el.textContent = \"BokehJS \" + root.Bokeh.version + \" successfully loaded.\";\n      }\n    } else if (Date.now() < root._bokeh_timeout) {\n      setTimeout(display_loaded, 100)\n    }\n  }\n\n\n  function run_callbacks() {\n    try {\n      root._bokeh_onload_callbacks.forEach(function(callback) {\n        if (callback != null)\n          callback();\n      });\n    } finally {\n      delete root._bokeh_onload_callbacks\n    }\n    console.debug(\"Bokeh: all callbacks have finished\");\n  }\n\n  function load_libs(css_urls, js_urls, callback) {\n    if (css_urls == null) css_urls = [];\n    if (js_urls == null) js_urls = [];\n\n    root._bokeh_onload_callbacks.push(callback);\n    if (root._bokeh_is_loading > 0) {\n      console.debug(\"Bokeh: BokehJS is being loaded, scheduling callback at\", now());\n      return null;\n    }\n    if (js_urls == null || js_urls.length === 0) {\n      run_callbacks();\n      return null;\n    }\n    console.debug(\"Bokeh: BokehJS not loaded, scheduling load and callback at\", now());\n    root._bokeh_is_loading = css_urls.length + js_urls.length;\n\n    function on_load() {\n      root._bokeh_is_loading--;\n      if (root._bokeh_is_loading === 0) {\n        console.debug(\"Bokeh: all BokehJS libraries/stylesheets loaded\");\n        run_callbacks()\n      }\n    }\n\n    function on_error() {\n      console.error(\"failed to load \" + url);\n    }\n\n    for (var i = 0; i < css_urls.length; i++) {\n      var url = css_urls[i];\n      const element = document.createElement(\"link\");\n      element.onload = on_load;\n      element.onerror = on_error;\n      element.rel = \"stylesheet\";\n      element.type = \"text/css\";\n      element.href = url;\n      console.debug(\"Bokeh: injecting link tag for BokehJS stylesheet: \", url);\n      document.body.appendChild(element);\n    }\n\n    for (var i = 0; i < js_urls.length; i++) {\n      var url = js_urls[i];\n      var element = document.createElement('script');\n      element.onload = on_load;\n      element.onerror = on_error;\n      element.async = false;\n      element.src = url;\n      console.debug(\"Bokeh: injecting script tag for BokehJS library: \", url);\n      document.head.appendChild(element);\n    }\n  };var element = document.getElementById(\"1001\");\n  if (element == null) {\n    console.error(\"Bokeh: ERROR: autoload.js configured with elementid '1001' but no matching script tag was found. \")\n    return false;\n  }\n\n  function inject_raw_css(css) {\n    const element = document.createElement(\"style\");\n    element.appendChild(document.createTextNode(css));\n    document.body.appendChild(element);\n  }\n\n  \n  var js_urls = [\"https://cdn.pydata.org/bokeh/release/bokeh-1.4.0.min.js\", \"https://cdn.pydata.org/bokeh/release/bokeh-widgets-1.4.0.min.js\", \"https://cdn.pydata.org/bokeh/release/bokeh-tables-1.4.0.min.js\", \"https://cdn.pydata.org/bokeh/release/bokeh-gl-1.4.0.min.js\"];\n  var css_urls = [];\n  \n\n  var inline_js = [\n    function(Bokeh) {\n      Bokeh.set_log_level(\"info\");\n    },\n    function(Bokeh) {\n    \n    \n    }\n  ];\n\n  function run_inline_js() {\n    \n    if (root.Bokeh !== undefined || force === true) {\n      \n    for (var i = 0; i < inline_js.length; i++) {\n      inline_js[i].call(root, root.Bokeh);\n    }\n    if (force === true) {\n        display_loaded();\n      }} else if (Date.now() < root._bokeh_timeout) {\n      setTimeout(run_inline_js, 100);\n    } else if (!root._bokeh_failed_load) {\n      console.log(\"Bokeh: BokehJS failed to load within specified timeout.\");\n      root._bokeh_failed_load = true;\n    } else if (force !== true) {\n      var cell = $(document.getElementById(\"1001\")).parents('.cell').data().cell;\n      cell.output_area.append_execute_result(NB_LOAD_WARNING)\n    }\n\n  }\n\n  if (root._bokeh_is_loading === 0) {\n    console.debug(\"Bokeh: BokehJS loaded, going straight to plotting\");\n    run_inline_js();\n  } else {\n    load_libs(css_urls, js_urls, function() {\n      console.debug(\"Bokeh: BokehJS plotting callback run at\", now());\n      run_inline_js();\n    });\n  }\n}(window));"
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "import pandas as pd\n",
    "from bokeh.plotting import figure, output_file, show,output_notebook\n",
    "output_notebook()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In this section, we define the function <code>make_dashboard</code>. \n",
    "You don't have to know how the function works, you should only care about the inputs. The function will produce a dashboard as well as an html file. You can then use this html file to share your dashboard. If you do not know what an html file is don't worry everything you need to know will be provided in the lab. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "def make_dashboard(x, gdp_change, unemployment, title, file_name):\n",
    "    output_file(file_name)\n",
    "    p = figure(title=title, x_axis_label='year', y_axis_label='%')\n",
    "    p.line(x.squeeze(), gdp_change.squeeze(), color=\"firebrick\", line_width=4, legend=\"% GDP change\")\n",
    "    p.line(x.squeeze(), unemployment.squeeze(), line_width=4, legend=\"% unemployed\")\n",
    "    show(p)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The dictionary  <code>links</code> contain the CSV files with all the data. The value for the key <code>GDP</code> is the file that contains the GDP data. The value for the key <code>unemployment</code> contains the unemployment data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "links={'GDP':'https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/PY0101EN/projects/coursera_project/clean_gdp.csv',\\\n",
    "       'unemployment':'https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/PY0101EN/projects/coursera_project/clean_unemployment.csv'}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h3 id=\"Section_2\"> Question 1: Create a dataframe that contains the GDP data and display the first five rows of the dataframe.</h3>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Use the dictionary <code>links</code> and the function <code>pd.read_csv</code> to create a Pandas dataframes that contains the GDP data."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<b>Hint: <code>links[\"GDP\"]</code> contains the path or name of the file.</b>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Type your code here\n",
    "csv_path=links[\"GDP\"]\n",
    "dfgdp = pd.read_csv(csv_path)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Use the method <code>head()</code> to display the first five rows of the GDP data, then take a screen-shot."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>date</th>\n",
       "      <th>level-current</th>\n",
       "      <th>level-chained</th>\n",
       "      <th>change-current</th>\n",
       "      <th>change-chained</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1948</td>\n",
       "      <td>274.8</td>\n",
       "      <td>2020.0</td>\n",
       "      <td>-0.7</td>\n",
       "      <td>-0.6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1949</td>\n",
       "      <td>272.8</td>\n",
       "      <td>2008.9</td>\n",
       "      <td>10.0</td>\n",
       "      <td>8.7</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1950</td>\n",
       "      <td>300.2</td>\n",
       "      <td>2184.0</td>\n",
       "      <td>15.7</td>\n",
       "      <td>8.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1951</td>\n",
       "      <td>347.3</td>\n",
       "      <td>2360.0</td>\n",
       "      <td>5.9</td>\n",
       "      <td>4.1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1952</td>\n",
       "      <td>367.7</td>\n",
       "      <td>2456.1</td>\n",
       "      <td>6.0</td>\n",
       "      <td>4.7</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   date  level-current  level-chained  change-current  change-chained\n",
       "0  1948          274.8         2020.0            -0.7            -0.6\n",
       "1  1949          272.8         2008.9            10.0             8.7\n",
       "2  1950          300.2         2184.0            15.7             8.0\n",
       "3  1951          347.3         2360.0             5.9             4.1\n",
       "4  1952          367.7         2456.1             6.0             4.7"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Type your code here\n",
    "dfgdp.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h3 id=\"Section_2\"> Question 2: Create a dataframe that contains the unemployment data. Display the first five rows of the dataframe. </h3>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Use the dictionary <code>links</code> and the function <code>pd.read_csv</code> to create a Pandas dataframes that contains the unemployment data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Type your code here\n",
    "csv_path=links[\"unemployment\"]\n",
    "dfun = pd.read_csv(csv_path)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Use the method <code>head()</code> to display the first five rows of the GDP data, then take a screen-shot."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>date</th>\n",
       "      <th>unemployment</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1948</td>\n",
       "      <td>3.750000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1949</td>\n",
       "      <td>6.050000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1950</td>\n",
       "      <td>5.208333</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1951</td>\n",
       "      <td>3.283333</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1952</td>\n",
       "      <td>3.025000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   date  unemployment\n",
       "0  1948      3.750000\n",
       "1  1949      6.050000\n",
       "2  1950      5.208333\n",
       "3  1951      3.283333\n",
       "4  1952      3.025000"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Type your code here\n",
    "dfun.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h3 id=\"Section_3\">Question 3: Display a dataframe where unemployment was greater than 8.5%. Take a screen-shot.</h3>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>date</th>\n",
       "      <th>unemployment</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>34</th>\n",
       "      <td>1982</td>\n",
       "      <td>9.708333</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>35</th>\n",
       "      <td>1983</td>\n",
       "      <td>9.600000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>61</th>\n",
       "      <td>2009</td>\n",
       "      <td>9.283333</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62</th>\n",
       "      <td>2010</td>\n",
       "      <td>9.608333</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>63</th>\n",
       "      <td>2011</td>\n",
       "      <td>8.933333</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    date  unemployment\n",
       "34  1982      9.708333\n",
       "35  1983      9.600000\n",
       "61  2009      9.283333\n",
       "62  2010      9.608333\n",
       "63  2011      8.933333"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Type your code here\n",
    "y = dfun[dfun['unemployment']>8.500000]\n",
    "y"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h3 id=\"Section_4\">Question 4: Use the function make_dashboard to make a dashboard</h3>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In this section, you will call the function  <code>make_dashboard</code> , to produce a dashboard. We will use the convention of giving each variable the same name as the function parameter."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Create a new dataframe with the column <code>'date'</code> called <code>x</code> from the dataframe that contains the GDP data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [],
   "source": [
    "x =dfgdp['date'] # Create your dataframe with column date\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Create a new dataframe with the column <code>'change-current' </code> called <code>gdp_change</code>  from the dataframe that contains the GDP data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [],
   "source": [
    "gdp_change = dfgdp['change-current']# Create your dataframe with column change-current"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Create a new dataframe with the column <code>'unemployment' </code> called <code>unemployment</code>  from the dataframe that contains the  unemployment data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [],
   "source": [
    "unemployment =dfun['unemployment'] # Create your dataframe with column unemployment"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Give your dashboard a string title, and assign it to the variable <code>title</code>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [],
   "source": [
    "title ='Analysis on unemployment data' # Give your dashboard a string title"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Finally, the function <code>make_dashboard</code> will output an <code>.html</code> in your direictory, just like a <code>csv</code> file. The name of the file is <code>\"index.html\"</code> and it will be stored in the varable  <code>file_name</code>."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [],
   "source": [
    "file_name = \"index.html\"  # E:\\Jupyter_Notebook"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Call the function <code>make_dashboard</code> , to produce a dashboard.  Assign the parameter values accordingly take a the <b>, take a screen shot of the dashboard and submit it</b>."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "BokehDeprecationWarning: 'legend' keyword is deprecated, use explicit 'legend_label', 'legend_field', or 'legend_group' keywords instead\n",
      "BokehDeprecationWarning: 'legend' keyword is deprecated, use explicit 'legend_label', 'legend_field', or 'legend_group' keywords instead\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "\n",
       "\n",
       "\n",
       "\n",
       "\n",
       "\n",
       "  <div class=\"bk-root\" id=\"5a562eac-416f-4bc2-b121-ee72031d53fd\" data-root-id=\"1244\"></div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "application/javascript": [
       "(function(root) {\n",
       "  function embed_document(root) {\n",
       "    \n",
       "  var docs_json = {\"0cecf961-fed1-4fe8-9372-ac75e9cca1bf\":{\"roots\":{\"references\":[{\"attributes\":{\"below\":[{\"id\":\"1255\",\"type\":\"LinearAxis\"}],\"center\":[{\"id\":\"1259\",\"type\":\"Grid\"},{\"id\":\"1264\",\"type\":\"Grid\"},{\"id\":\"1289\",\"type\":\"Legend\"}],\"left\":[{\"id\":\"1260\",\"type\":\"LinearAxis\"}],\"renderers\":[{\"id\":\"1281\",\"type\":\"GlyphRenderer\"},{\"id\":\"1294\",\"type\":\"GlyphRenderer\"}],\"title\":{\"id\":\"1245\",\"type\":\"Title\"},\"toolbar\":{\"id\":\"1271\",\"type\":\"Toolbar\"},\"x_range\":{\"id\":\"1247\",\"type\":\"DataRange1d\"},\"x_scale\":{\"id\":\"1251\",\"type\":\"LinearScale\"},\"y_range\":{\"id\":\"1249\",\"type\":\"DataRange1d\"},\"y_scale\":{\"id\":\"1253\",\"type\":\"LinearScale\"}},\"id\":\"1244\",\"subtype\":\"Figure\",\"type\":\"Plot\"},{\"attributes\":{\"axis_label\":\"%\",\"formatter\":{\"id\":\"1287\",\"type\":\"BasicTickFormatter\"},\"ticker\":{\"id\":\"1261\",\"type\":\"BasicTicker\"}},\"id\":\"1260\",\"type\":\"LinearAxis\"},{\"attributes\":{\"line_alpha\":0.1,\"line_color\":\"#1f77b4\",\"line_width\":4,\"x\":{\"field\":\"x\"},\"y\":{\"field\":\"y\"}},\"id\":\"1293\",\"type\":\"Line\"},{\"attributes\":{\"bottom_units\":\"screen\",\"fill_alpha\":{\"value\":0.5},\"fill_color\":{\"value\":\"lightgrey\"},\"left_units\":\"screen\",\"level\":\"overlay\",\"line_alpha\":{\"value\":1.0},\"line_color\":{\"value\":\"black\"},\"line_dash\":[4,4],\"line_width\":{\"value\":2},\"render_mode\":\"css\",\"right_units\":\"screen\",\"top_units\":\"screen\"},\"id\":\"1288\",\"type\":\"BoxAnnotation\"},{\"attributes\":{},\"id\":\"1251\",\"type\":\"LinearScale\"},{\"attributes\":{},\"id\":\"1285\",\"type\":\"BasicTickFormatter\"},{\"attributes\":{},\"id\":\"1302\",\"type\":\"Selection\"},{\"attributes\":{\"data_source\":{\"id\":\"1278\",\"type\":\"ColumnDataSource\"},\"glyph\":{\"id\":\"1279\",\"type\":\"Line\"},\"hover_glyph\":null,\"muted_glyph\":null,\"nonselection_glyph\":{\"id\":\"1280\",\"type\":\"Line\"},\"selection_glyph\":null,\"view\":{\"id\":\"1282\",\"type\":\"CDSView\"}},\"id\":\"1281\",\"type\":\"GlyphRenderer\"},{\"attributes\":{\"text\":\"Analysis on unemployment data\"},\"id\":\"1245\",\"type\":\"Title\"},{\"attributes\":{},\"id\":\"1261\",\"type\":\"BasicTicker\"},{\"attributes\":{\"callback\":null},\"id\":\"1247\",\"type\":\"DataRange1d\"},{\"attributes\":{\"label\":{\"value\":\"% unemployed\"},\"renderers\":[{\"id\":\"1294\",\"type\":\"GlyphRenderer\"}]},\"id\":\"1304\",\"type\":\"LegendItem\"},{\"attributes\":{},\"id\":\"1324\",\"type\":\"UnionRenderers\"},{\"attributes\":{},\"id\":\"1256\",\"type\":\"BasicTicker\"},{\"attributes\":{\"source\":{\"id\":\"1291\",\"type\":\"ColumnDataSource\"}},\"id\":\"1295\",\"type\":\"CDSView\"},{\"attributes\":{\"line_color\":\"#1f77b4\",\"line_width\":4,\"x\":{\"field\":\"x\"},\"y\":{\"field\":\"y\"}},\"id\":\"1292\",\"type\":\"Line\"},{\"attributes\":{},\"id\":\"1323\",\"type\":\"Selection\"},{\"attributes\":{},\"id\":\"1265\",\"type\":\"PanTool\"},{\"attributes\":{},\"id\":\"1270\",\"type\":\"HelpTool\"},{\"attributes\":{\"axis_label\":\"year\",\"formatter\":{\"id\":\"1285\",\"type\":\"BasicTickFormatter\"},\"ticker\":{\"id\":\"1256\",\"type\":\"BasicTicker\"}},\"id\":\"1255\",\"type\":\"LinearAxis\"},{\"attributes\":{},\"id\":\"1287\",\"type\":\"BasicTickFormatter\"},{\"attributes\":{\"active_drag\":\"auto\",\"active_inspect\":\"auto\",\"active_multi\":null,\"active_scroll\":\"auto\",\"active_tap\":\"auto\",\"tools\":[{\"id\":\"1265\",\"type\":\"PanTool\"},{\"id\":\"1266\",\"type\":\"WheelZoomTool\"},{\"id\":\"1267\",\"type\":\"BoxZoomTool\"},{\"id\":\"1268\",\"type\":\"SaveTool\"},{\"id\":\"1269\",\"type\":\"ResetTool\"},{\"id\":\"1270\",\"type\":\"HelpTool\"}]},\"id\":\"1271\",\"type\":\"Toolbar\"},{\"attributes\":{},\"id\":\"1303\",\"type\":\"UnionRenderers\"},{\"attributes\":{\"items\":[{\"id\":\"1290\",\"type\":\"LegendItem\"},{\"id\":\"1304\",\"type\":\"LegendItem\"}]},\"id\":\"1289\",\"type\":\"Legend\"},{\"attributes\":{\"data_source\":{\"id\":\"1291\",\"type\":\"ColumnDataSource\"},\"glyph\":{\"id\":\"1292\",\"type\":\"Line\"},\"hover_glyph\":null,\"muted_glyph\":null,\"nonselection_glyph\":{\"id\":\"1293\",\"type\":\"Line\"},\"selection_glyph\":null,\"view\":{\"id\":\"1295\",\"type\":\"CDSView\"}},\"id\":\"1294\",\"type\":\"GlyphRenderer\"},{\"attributes\":{},\"id\":\"1269\",\"type\":\"ResetTool\"},{\"attributes\":{\"callback\":null,\"data\":{\"x\":[1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],\"y\":{\"__ndarray__\":\"AAAAAAAADkAzMzMzMzMYQFVVVVVV1RRARURERERECkA0MzMzMzMIQGdmZmZmZgdA3d3d3d1dFkB4d3d3d3cRQAAAAAAAgBBAMzMzMzMzEUDd3d3d3V0bQM3MzMzMzBVArKqqqqoqFkBERERERMQaQEVERERERBZAEhERERGRFkAiIiIiIqIUQImIiIiICBJAVVVVVVVVDkC5u7u7u7sOQHd3d3d3dwxA7+7u7u7uC0Dv7u7u7u4TQM3MzMzMzBdAZ2ZmZmZmFkDu7u7u7m4TQBIRERERkRZAMzMzMzPzIEDLzMzMzMweQDUzMzMzMxxAREREREREGEBnZmZmZmYXQDUzMzMzsxxAeHd3d3d3HkCqqqqqqmojQDMzMzMzMyNAiYiIiIgIHkBERERERMQcQAAAAAAAABxANTMzMzOzGEB3d3d3d/cVQIiIiIiICBVAd3d3d3d3FkBlZmZmZmYbQHh3d3d39x1AISIiIiKiG0BnZmZmZmYYQN/d3d3dXRZAIyIiIiKiFUBERERERMQTQAAAAAAAABJA393d3d3dEEC5u7u7u7sPQHd3d3d39xJAIyIiIiIiF0B4d3d3d/cXQKyqqqqqKhZAVVVVVVVVFEDv7u7u7m4SQHh3d3d3dxJAMzMzMzMzF0AREREREZEiQHd3d3d3NyNA3t3d3d3dIUBnZmZmZiYgQO/u7u7ubh1AIyIiIiKiGECamZmZmRkVQAAAAAAAgBNA\",\"dtype\":\"float64\",\"shape\":[69]}},\"selected\":{\"id\":\"1323\",\"type\":\"Selection\"},\"selection_policy\":{\"id\":\"1324\",\"type\":\"UnionRenderers\"}},\"id\":\"1291\",\"type\":\"ColumnDataSource\"},{\"attributes\":{\"overlay\":{\"id\":\"1288\",\"type\":\"BoxAnnotation\"}},\"id\":\"1267\",\"type\":\"BoxZoomTool\"},{\"attributes\":{\"line_color\":\"firebrick\",\"line_width\":4,\"x\":{\"field\":\"x\"},\"y\":{\"field\":\"y\"}},\"id\":\"1279\",\"type\":\"Line\"},{\"attributes\":{\"label\":{\"value\":\"% GDP change\"},\"renderers\":[{\"id\":\"1281\",\"type\":\"GlyphRenderer\"}]},\"id\":\"1290\",\"type\":\"LegendItem\"},{\"attributes\":{},\"id\":\"1266\",\"type\":\"WheelZoomTool\"},{\"attributes\":{\"callback\":null},\"id\":\"1249\",\"type\":\"DataRange1d\"},{\"attributes\":{\"callback\":null,\"data\":{\"x\":[1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],\"y\":{\"__ndarray__\":\"ZmZmZmZm5r8AAAAAAAAkQGZmZmZmZi9AmpmZmZmZF0AAAAAAAAAYQDMzMzMzM9M/zczMzMzMIUBmZmZmZmYWQAAAAAAAABZAAAAAAAAA+D/NzMzMzMwgQAAAAAAAABBAmpmZmZmZDUCamZmZmZkdQGZmZmZmZhZAmpmZmZmZHUDNzMzMzMwgQDMzMzMzMyNAzczMzMzMFkDNzMzMzMwiQGZmZmZmZiBAAAAAAAAAFkAAAAAAAAAhQJqZmZmZmSNAzczMzMzMJkDNzMzMzMwgQAAAAAAAACJAZmZmZmZmJkAzMzMzMzMmQAAAAAAAACpAZmZmZmZmJ0CamZmZmZkhQGZmZmZmZihAMzMzMzMzEUBmZmZmZmYhQDMzMzMzMyZAAAAAAAAAHkAAAAAAAAAWQAAAAAAAABhAmpmZmZmZH0DNzMzMzMweQM3MzMzMzBZAZmZmZmZmCkCamZmZmZkXQM3MzMzMzBRAMzMzMzMzGUAzMzMzMzMTQM3MzMzMzBZAzczMzMzMGEDNzMzMzMwWQDMzMzMzMxlAAAAAAAAAGkCamZmZmZkJQDMzMzMzMwtAMzMzMzMzE0BmZmZmZmYaQM3MzMzMzBpAAAAAAAAAGEBmZmZmZmYSQM3MzMzMzPw/zczMzMzM/L9mZmZmZmYOQJqZmZmZmQ1AzczMzMzMEEDNzMzMzMwMQJqZmZmZmRFAAAAAAAAAEECamZmZmZkFQM3MzMzMzBBA\",\"dtype\":\"float64\",\"shape\":[69]}},\"selected\":{\"id\":\"1302\",\"type\":\"Selection\"},\"selection_policy\":{\"id\":\"1303\",\"type\":\"UnionRenderers\"}},\"id\":\"1278\",\"type\":\"ColumnDataSource\"},{\"attributes\":{},\"id\":\"1268\",\"type\":\"SaveTool\"},{\"attributes\":{\"ticker\":{\"id\":\"1256\",\"type\":\"BasicTicker\"}},\"id\":\"1259\",\"type\":\"Grid\"},{\"attributes\":{\"dimension\":1,\"ticker\":{\"id\":\"1261\",\"type\":\"BasicTicker\"}},\"id\":\"1264\",\"type\":\"Grid\"},{\"attributes\":{},\"id\":\"1253\",\"type\":\"LinearScale\"},{\"attributes\":{\"line_alpha\":0.1,\"line_color\":\"#1f77b4\",\"line_width\":4,\"x\":{\"field\":\"x\"},\"y\":{\"field\":\"y\"}},\"id\":\"1280\",\"type\":\"Line\"},{\"attributes\":{\"source\":{\"id\":\"1278\",\"type\":\"ColumnDataSource\"}},\"id\":\"1282\",\"type\":\"CDSView\"}],\"root_ids\":[\"1244\"]},\"title\":\"Bokeh Application\",\"version\":\"1.4.0\"}};\n",
       "  var render_items = [{\"docid\":\"0cecf961-fed1-4fe8-9372-ac75e9cca1bf\",\"roots\":{\"1244\":\"5a562eac-416f-4bc2-b121-ee72031d53fd\"}}];\n",
       "  root.Bokeh.embed.embed_items_notebook(docs_json, render_items);\n",
       "\n",
       "  }\n",
       "  if (root.Bokeh !== undefined) {\n",
       "    embed_document(root);\n",
       "  } else {\n",
       "    var attempts = 0;\n",
       "    var timer = setInterval(function(root) {\n",
       "      if (root.Bokeh !== undefined) {\n",
       "        clearInterval(timer);\n",
       "        embed_document(root);\n",
       "      } else {\n",
       "        attempts++;\n",
       "        if (attempts > 100) {\n",
       "          clearInterval(timer);\n",
       "          console.log(\"Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing\");\n",
       "        }\n",
       "      }\n",
       "    }, 10, root)\n",
       "  }\n",
       "})(window);"
      ],
      "application/vnd.bokehjs_exec.v0+json": ""
     },
     "metadata": {
      "application/vnd.bokehjs_exec.v0+json": {
       "id": "1244"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Fill up the parameters in the following function:\n",
    "make_dashboard(x=x, gdp_change=gdp_change, unemployment=unemployment, title=title, file_name=file_name)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h3 id=\"Section_5\"> <b>(Optional not marked)</b>Save the dashboard on IBM cloud and display it  </h3>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "From the tutorial <i>PROVISIONING AN OBJECT STORAGE INSTANCE ON IBM CLOUD</i> copy the JSON object containing the credentials you created. You’ll want to store everything you see in a credentials variable like the one below (obviously, replace the placeholder values with your own). Take special note of your <code>access_key_id</code> and <code>secret_access_key</code>. <b>Do not delete <code># @hidden_cell </code> as this will not allow people to see your credentials when you share your notebook. </b>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<code>\n",
    "credentials = {<br>\n",
    " &nbsp; \"apikey\": \"your-api-key\",<br>\n",
    " &nbsp; \"cos_hmac_keys\": {<br>\n",
    " &nbsp;  \"access_key_id\": \"your-access-key-here\", <br>\n",
    " &nbsp;   \"secret_access_key\": \"your-secret-access-key-here\"<br>\n",
    " &nbsp; },<br>\n",
    "</code>\n",
    "<code>\n",
    "   &nbsp;\"endpoints\": \"your-endpoints\",<br>\n",
    " &nbsp; \"iam_apikey_description\": \"your-iam_apikey_description\",<br>\n",
    " &nbsp; \"iam_apikey_name\": \"your-iam_apikey_name\",<br>\n",
    " &nbsp; \"iam_role_crn\": \"your-iam_apikey_name\",<br>\n",
    " &nbsp;  \"iam_serviceid_crn\": \"your-iam_serviceid_crn\",<br>\n",
    " &nbsp;\"resource_instance_id\": \"your-resource_instance_id\"<br>\n",
    "}\n",
    "</code>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [],
   "source": [
    "# @hidden_cell\n",
    "#"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "You will need the endpoint make sure the setting are the same as <i> PROVISIONING AN OBJECT STORAGE INSTANCE ON IBM CLOUD </i> assign the name of your bucket to the variable  <code>bucket_name </code> "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [],
   "source": [
    "endpoint = 's3.ap.cloud-object-storage.appdomain.cloud'"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "From the tutorial <i> PROVISIONING AN OBJECT STORAGE INSTANCE ON IBM CLOUD </i> assign the name of your bucket to the variable  <code>bucket_name </code> "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [],
   "source": [
    "bucket_name =\"cloud-object-storage-dh-cos-standard-mew\"\n",
    " # Type your bucket name on IBM Cloud"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can access IBM Cloud Object Storage with Python useing the <code>boto3</code> library, which we’ll import below:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "ename": "ModuleNotFoundError",
     "evalue": "No module named 'boto3'",
     "output_type": "error",
     "traceback": [
      "\u001b[1;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[1;31mModuleNotFoundError\u001b[0m                       Traceback (most recent call last)",
      "\u001b[1;32m<ipython-input-40-5c43c86c018e>\u001b[0m in \u001b[0;36m<module>\u001b[1;34m\u001b[0m\n\u001b[1;32m----> 1\u001b[1;33m \u001b[1;32mimport\u001b[0m \u001b[0mboto3\u001b[0m\u001b[1;33m\u001b[0m\u001b[1;33m\u001b[0m\u001b[0m\n\u001b[0m",
      "\u001b[1;31mModuleNotFoundError\u001b[0m: No module named 'boto3'"
     ]
    }
   ],
   "source": [
    "import boto3"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can interact with IBM Cloud Object Storage through a <code>boto3</code> resource object."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "resource = boto3.resource(\n",
    "    's3',\n",
    "    aws_access_key_id = credentials[\"cos_hmac_keys\"]['access_key_id'],\n",
    "    aws_secret_access_key = credentials[\"cos_hmac_keys\"][\"secret_access_key\"],\n",
    "    endpoint_url = endpoint,\n",
    ")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We are going to use  <code>open</code> to create a file object. To get the path of the file, you are going to concatenate the name of the file stored in the variable <code>file_name</code>. The directory stored in the variable directory using the <code>+</code> operator and assign it to the variable \n",
    "<code>html_path</code>. We will use the function <code>getcwd()</code> to find current the working directory."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import os\n",
    "\n",
    "directory = os.getcwd()\n",
    "html_path = directory + \"/\" + file_name"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now you must read the html file, use the function <code>f = open(html_path, mode)</code> to create a file object and assign it to the variable <code>f</code>. The parameter <code>file</code> should be the variable <code>html_path</code>, the mode should be <code>\"r\"</code> for read. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Type your code here"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "To load your dataset into the bucket we will use the method <code>put_object</code>, you must set the parameter name to the name of the bucket, the parameter <code>Key</code> should be the name of the HTML file and the value for the parameter Body  should be set to <code>f.read()</code>."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Fill up the parameters in the following function:\n",
    "# resource.Bucket(name=).put_object(Key=, Body=)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In the dictionary <code>Params</code> provide the bucket name  as the value for the key <i>'Bucket'</i>. Also for the value of the key <i>'Key'</i> add the name of the <code>html</code> file, both values should be strings."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Fill in the value for each key\n",
    "# Params = {'Bucket': ,'Key': }"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The following lines of code will generate a URL to share your dashboard. The URL only last seven days, but don't worry you will get full marks if the URL is visible in your notebook.  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import sys\n",
    "time = 7*24*60**2\n",
    "client = boto3.client(\n",
    "    's3',\n",
    "    aws_access_key_id = credentials[\"cos_hmac_keys\"]['access_key_id'],\n",
    "    aws_secret_access_key = credentials[\"cos_hmac_keys\"][\"secret_access_key\"],\n",
    "    endpoint_url=endpoint,\n",
    "\n",
    ")\n",
    "url = client.generate_presigned_url('get_object',Params=Params,ExpiresIn=time)\n",
    "print(url)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2 id=\"Section_5\">  How to submit </h2>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<p>Once you complete your notebook you will have to share it to be marked. Select the icon on the top right a marked in red in the image below, a dialogue box should open, select the option all&nbsp;content excluding sensitive code cells.</p>\n",
    "\n",
    "<p><img height=\"440\" width=\"700\" src=\"https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/PY0101EN/projects/EdX/ReadMe%20files/share_noteook1.png\" alt=\"share notebook\" /></p>\n",
    "<p></p>\n",
    "\n",
    "<p>You can then share the notebook&nbsp; via a&nbsp; URL by scrolling down as shown in the following image:</p>\n",
    "<p style=\"text-align: center;\"> <img height=\"308\" width=\"350\" src=\"https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/PY0101EN/projects/EdX/ReadMe%20files/link2.png\"  alt=\"share notebook\" /> </p>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<hr>\n",
    "<p>Copyright &copy; 2019 IBM Developer Skills Network. This notebook and its source code are released under the terms of the <a href=\"https://cognitiveclass.ai/mit-license/\">MIT License</a>.</p>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2>About the Authors:</h2> \n",
    "\n",
    "<a href=\"https://www.linkedin.com/in/joseph-s-50398b136/\">Joseph Santarcangelo</a> has a PhD in Electrical Engineering, his research focused on using machine learning, signal processing, and computer vision to determine how videos impact human cognition. Joseph has been working for IBM since he completed his PhD.\n",
    "<p>\n",
    "Other contributors: <a href=\"https://www.linkedin.com/in/yi-leng-yao-84451275/\">Yi leng Yao</a>, <a href=\"www.linkedin.com/in/jiahui-mavis-zhou-a4537814a\">Mavis Zhou</a> \n",
    "</p>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<h2>References :</h2> "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<ul>\n",
    " <il>\n",
    "     1) <a href=\"https://research.stlouisfed.org/\">Economic Research at the St. Louis Fed </a>:<a href=\"https://fred.stlouisfed.org/series/UNRATE/\"> Civilian Unemployment Rate</a>\n",
    "   </il>   \n",
    "    <p>\n",
    "     <il>\n",
    "    2) <a href=\"https://github.com/datasets\">Data Packaged Core Datasets\n",
    "       </a>\n",
    "   </il> \n",
    "    </p>\n",
    "    \n",
    "</ul>\n",
    "</div>"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.6"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
