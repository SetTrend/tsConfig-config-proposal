# Improved TypeScript configuration reference web page

## Working Proposal Example

Find the living proposal example here: https://settrend.github.io/tsConfig-config-proposal/

## Synopsis

When visiting the original TypeScript [tsconfig reference](https://www.typescriptlang.org/tsconfig/) documentation web page, the first impression right away is that the page is Brobdingnagian, overweight and overwhelming. It has all the vast amount of configuration information crammed into a single, huge page that's hard to scroll and hard to survey.

From my personal point of view, it's too hard to find pertinent information therein in a timely manner. This is particularly true for daily work, where configuring TSC is not supposed to become  a full-time task.

I propose to add search and filter options to the page as well as a new page layout, supposed to resemble the powerful, simple and easy to grasp File Explorer design.

See this TypeScript Website issue I filed: https://github.com/microsoft/TypeScript-Website/issues/3377

# Proposal

Using the original [tsconfig reference](https://www.typescriptlang.org/tsconfig/) documentation web page, I created a **simple [mock tsconfig reference](index.html) page**, visualizing my proposal:

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <img src=".doc/Improve%20tsconfig%20webpage.gif" alt="Improve tsconfig web page" width="66%"/>

<br/>

You can find the living example here: https://settrend.github.io/tsConfig-config-proposal/

# Proposal Overview

The changes are simple and easy to implement:

1. Re-arranging configuration property hyperlinks and content into two flex columns, restricting their max height to match the browser window height:

   ```html
   <div style="display: flex;">

     <div id="nodeList" style="flex: 0 0 20em; overflow: auto;">
         <!-- configuration property hyperlinks -->
     </div>

     <div style="flex: auto; overflow: auto;">
         <!-- configuration properties' content -->
     </div>

   </div>
   ```

1. Adding a search bar to the top of the left flex block, along with the corresponding JavaScript:

   ```html
    <div id="listInputRow">
      <div>
          🔍
          <input id="filter" name="filter" type="search" style="margin:.2ex;" />
          <button id="submit">⎚</button>
      </div>
    </div>

    <script>
        function filter(str)
        {
            const liElems = document.getElementById("nodeList").querySelectorAll("li");

            str = str.toLowerCase();
            liElems.forEach(li => li.className = str && !li.innerText.toLowerCase().includes(str) ? "hidden" : "");
        }

        function clear() { filter(fltInput.value = ""); }

        const fltInput = document.getElementById("filter");
        const sbmInput = document.getElementById("submit");

        fltInput.addEventListener("input", ev => filter(fltInput.value));
        fltInput.addEventListener("keyup", ev => { if (ev.key.substring(0, 3) === "Esc") clear(); });
        sbmInput.addEventListener("click", ev => clear());
    </script>
   ```

1. When activating any of the configuration property hyperlinks, show filtered content in the right flex block by hiding all sections not addressed by the hyperlink:

   ```html
    <script>
        document.querySelectorAll("#nodeList a").forEach(a => a.addEventListener("click", ev =>
        {
            document.querySelectorAll("section").forEach(s => s.classList.add("hidden"));
            document.querySelector(new URL(ev.target.href).hash).parentElement.parentElement.classList.remove("hidden");
        }
        ));
    </script>
   ```

1. Add an informative tooltip to each of the configuration property hyperlinks:

   ```html
    <script>
      document.querySelectorAll("#nodeList a").forEach(a => a.title = document.getElementById(new URL(a.href).hash.substring(1))?.parentElement?.parentElement?.querySelector(".markdown p")?.innerText);
    </script>
   ```