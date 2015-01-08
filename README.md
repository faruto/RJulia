RJulia
======

RJula provides an interface between R and Julia. It allows a user to run a script in Julia from R, and maps objects between the two languages.

It currently supports use on Linux and Windows from the console,but build on Windows only for advance users.

Install
-------------

1. Install julia v0.3 or the current master branch; R version >=3.1.0.

  **(WARNING: due to https://github.com/JuliaLang/julia/issues/9117 , rjulia compile fail under julia 0.3.4. to slove this,there are two way:
   1. use julia 0.3.3 or 0.4.0-dev to compile,install and use rjulia.
   2. use julia 0.3.3 to compile and install rjula ,after that user can upgrade to julia 0.3.4
   )**

2. add juliainstalldir/bin into your system PATH variable

3. Compile Rjulia.

4. If you want to be able to type map R or Julia objects that contain NA values or factor or Data Frame, Julia packages DataArrays and DataFrames must be installed.

5. Windows user please download built binary package from https://github.com/armgong/RJulia/releases  **(Caution: use master branch build first, nextgen branch build not stable)**

Simple Run it
------------- 
library(rjulia)

julia_init() #**(will auto find your julia home)**

julia_eval("1+1")

Demo
-------------

please see .R files in demo dir 

Doc
-------------
to be done

**Important Information**
-------------
now develop on two branch,master branch is stable version,nextgen branch is development version.nextgen branch uses pure julia C API to call julia function, don't mix julia script and c code,**so it  fast than master branch and use less memory ,but  less stable**.

if you want use nextgen branch, please patch your DataFrames package, add one function into it:
```julia
# Initialize an empty DataFrame with specific eltypes and names and whether is pooled data array
function DataFrame(column_eltypes::Vector, cnames::Vector,ispda::Vector, nrows::Integer)
    p = length(column_eltypes)
    columns = Array(Any, p)
    for j in 1:p
      if ispda[j]
        columns[j] = PooledDataArray(column_eltypes[j], nrows)
      else
        columns[j] = DataArray(column_eltypes[j], nrows)
      end  
    end
    return DataFrame(columns, Index(cnames))
end



Yu Gong
