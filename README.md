# NeutriumJS.Steam

## Introduction

NeutriumJS Steam is a stand alone javascript implementation of the [IAPWS](http://www.iapws.org/) formulations of the thermodynamic properties of water and steam. The IAPWS papers implemented in NeutriumJS.Steam are as follows:

- [Revised Release on the IAPWS Industrial Formulation 1997 for the Thermodynamic Properties of Water and Steam 2007](http://www.iapws.org/relguide/IF97-Rev.html)
- [Revised Supplementary Release on Backward Equations for Pressure as a Function of Enthalpy and Entropy p(h,s) for Regions 1 and 2 of the IAPWS Industrial Formulation 1997 for the Thermodynamic Properties of Water and Steam](http://www.iapws.org/relguide/Supp-PHS12-2014.pdf)
- [Revised Supplementary Release on Backward Equations for the Functions T(p,h), v(p,h) and T(p,s), v(p,s) for Region 3 of the IAPWS Industrial Formulation 1997 for the Thermodynamic Properties of Water and Steam](http://www.iapws.org/relguide/Supp-Tv\(ph,ps\)3-2014.pdf)
- [Revised Supplementary Release on Backward Equations p(h,s) for Region 3, Equations as a Function of h and s for the Region Boundaries, and an Equation Tsat(h,s) for Region 4 of the IAPWS Industrial Formulation 1997 for the Thermodynamic Properties of Water and Steam](http://www.iapws.org/relguide/Supp-phs3-2014.pdf)
- [Revised Supplementary Release on Backward Equations for Specific Volume as a Function of Pressure and Temperature v(p,T) for Region 3 of the IAPWS Industrial Formulation 1997 for the Thermodynamic Properties of Water and Steam](http://www.iapws.org/relguide/Supp-VPT3-2014.pdf)
- [Revised Release on Surface Tension of Ordinary Water Substance 2014](http://www.iapws.org/relguide/Surf-H2O-2014.pdf)
- [Release on the IAPWS Formulation 2011 for the Thermal Conductivity of Ordinary Water Substance](http://www.iapws.org/relguide/ThCond.pdf)
- [Release on the IAPWS Formulation 2008 for the Viscosity of Ordinary Water Substance](http://www.iapws.org/relguide/visc.pdf)
- [Release on the Ionization Constant of H2O 2007](http://www.iapws.org/relguide/Ionization.pdf)

For specific details on range of applicability for the IAPWS please refer to the [Revised Release on the IAPWS Industrial Formulation 1997 for the Thermodynamic Properties of Water and Steam 2007](http://www.iapws.org/relguide/IF97-Rev.html).

## Getting Started

### Adding NeutriumJS.Steam

#### Bower.io

You can install NeutriumJS Steam using bower.

	bower install neutriumjs-steam

#### npm

You can add NeutriumJS.Steam to your project using npm by:

	npm install neutriumjs.steam --save

### Standalone

If your project is not using bower you can use the compiled and minified source which is found at:

	dist/neutriumJS.steam.min.js

## Including the library

The NeutriumJS.Steam library should be included into your page using  the following:

	<script charset="utf-8" src="path-to-lib/neutriumJS.steam.min.js"></script>

Note the use of charset="utf-8" is important, particularly if using un-minified code as the codebase makes use of unicode character variable names. 

### Limiting Library Weight

NeutriumJS.Steam is divided into base, pressure-temperature (PT), pressure-entropy (PS), pressure-enthalpy (PH) and enthalpy-entropy (HS) modules. If you only require a subset of this functionality you can exclude modules to reduce the weight of the library. There are a number of builds in the ./dist folder that provide selected subsets of the functionality, however you can also create your own subset as follows:


At a bare minimum you must include the following files which will provide the functionality to calculate steam properties using pressure and temperature (these are the dependencies of all sub modules):

	src/NeutriumJS.Steam.js
	src/NeutriumJS.Steam_PT.js
	
To enable calculation of steam properties using pressure-entropy or pressure-enthalpy in addition to the base requirements above you need to include either/both of the following files as appropriate:

	src/NeutriumJS.Steam_PS.js
	src/NeutriumJS.Steam_PH.js
	
Lastly if you wanted to enable calculations based on enthalpy-entropy (HS) you need to include the following files in addition to the base requirements (note the dependency on the PH module):
 	
 	src/NeutriumJS.Steam_PH.js
	src/NeutriumJS.Steam_HS.js

If you need further clarification of module dependencies just refer to the UMD definition at the start of each source file.

## Calculating Steam Properties

IAPWS provides four methods to calculate the properties of steam and water using combinations of pressure (in MPa), temperature (K), enthalpy (kJ/kg.K) and entropy (kJ/K.kg). In NeutriumJS Steam these four functions  are listed as follows:

	NeutriumJS.Steam.PT.solve(P, T);	// Calculate properties from pressure and temperature
	NeutriumJS.Steam.PH.solve(P, H);	// Calculate properties from pressure and enthalpy
	NeutriumJS.Steam.PS.solve(P, S);	// Calculate properties from pressure and entropy
	NeutriumJS.Steam.HS.solve(H, S);	// Calculate properties from enthalpy and entropy


### Return Values

If your specified values lie within the applicable range for the IAWPS formulations you will be return an object containing the following properties:

	{
		p, 		// Pressure, p, MPa
		t, 		// Temperature, t, K
		v, 		// Specific volume, v, m^3/kg
		rho,	// Density, rho, kg/m^3
		u,		// Specific internal energy, u, kJ/kg
		s,		// Specific entropy, s, kJ/kg
		h, 		// Specific enthalpy, h, kJ/kg.K
		cp,		// Specific isobaric heat capacity, Cp kJ/kg.K
		cv,		// Specific isochoric heat capacity, Cv
		w,		// Speed of Sound, w, m/s
		mu,		// Viscosity cP,
		k,		// Thermal Conductivity W/m.K
		sig,	// Surface Tension mN/m
		epsilon,// Dielectric constant
		ic		// Ionisation constant
	}

If you try and calculate the properties outside the range of applicability as specified by IAWPS NeutriumJS Steam will return null.

## Optional Neutrium Convert Support

NeutriumJS.steam has optional support for the [NeutriumJS.convert](https://github.com/NativeDynamics/NeutriumJS.convert) module. Just include it before the steam modules:

	<script charset="utf-8" src="path-to-lib/neutriumJS.convert.min.js"></script> 
	<script charset="utf-8" src="path-to-lib/neutriumJS.steam.min.js"></script>

Then you can get NeutriumJS.steam to return results as Qty objects:

	var result = NeutriumJS.steam.PT.solve(3, 300).asQty();
	
This will allow you to easily convert each property as required:

	var psi = result.P.to('psi');

See the  the NeutriumJS.convert [readme](https://github.com/NativeDynamics/NeutriumJS.convert/blob/master/README.md) for more info.

## Testing

NeutriumJS Steam is currently tested using all applicable tests provided in the IAWPS papers listed above. To run the tests, after cloning the repo install all npm and bower dependencies then run the default gulp task.

## Donations

NeutriumJS is free software, but you can support the developers by [donating here](https://neutrium.net/donate/).

## Release Notes

| Version | Notes |
|:-------:|:------|
| 1.0.0	  | Initial Release |
| 1.0.5   | Add UMD definition |
| 1.1.0   | Optional NeutriumJS.convert support |
| 1.1.1	  | Change P and T keys to lower case |

## License 

[Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/legalcode)