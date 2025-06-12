---
title: "Examples of GENESIS Input File"
excerpt: "How to write control files for GENESIS."
last_modified_at: 2025-05-08T11:59:26+09:00
layout: single
toc: false
sidebar:
  nav: sidebar-basic
---

## Example control files for atdyn/spdyn

- Recommended main control parameters
  - [For GENESIS ver. 1.4 or later](/examples/recommended_parameters/)

- Molecular dynamics (MD) and energy minimization
  - [Energy minimization with positional restraints on protein heavy atoms](/examples/energy_minimization_with_positional_restraints_on_protein_heavy_atoms/)
  - [MD simulation in the NVE ensemble with the CHARMM, AMBER, and coarse-grained force fields](/examples/md_simulation_in_the_nve_ensemble_with_the_charmm__amber__and_coarse-grained_force_fields/)
  - [MD simulation in the NVT ensemble with Langevin, Berendsen, and Bussi thermostat](/examples/md_simulation_in_the_nvt_ensemble_with_langevin__berendsen__and_bussi_thermostat/)
  - [MD simulation in the NPT ensemble with Langevin, Berendsen, and Bussi thermostat and barostat](/examples/md_simulation_in_the_npt_ensemble_with_langevin__berendsen__and_bussi_thermostat_and_barostat/)
  - [MD simulation of a biological membrane in the NPT, NPAT, and NPγT ensembles](/examples/md_simulation_of_a_biological_membrane_in_the_npt__npat__and_npγt_ensembles/)
  - [MD simulation of a small peptide in vacuum in the canonical ensemble (`atdyn` only)](/examples/md_simulation_of_a_small_peptide_in_vacuum_in_the_canonical_ensemble_atdyn_only/)

- Enhanced-sampling simulation
  - [Temperature REMD simulation in the NVT and NPT ensembles](/examples/temperature_REMD_simulation_in_NVT_NPT_ensembles/)
  - [Pressure REMD simulation in the NPT ensemble](/examples/pressure_remd_simulation_in_the_npt_ensemble/)
  - [Surface-tension REMD simulation for a membrane system](/examples/surface-tension_remd_simulation_for_a_membrane_system/)
  - [Replica-exchange umbrella sampling (REUS or Hamiltonian REMD) using a distance restraint](/examples/sample_of_reus/)
  - [Two-dimensional REMD (T-REMD/REUS) simulation](/examples/two-dimensional_remd_t-remd_reus_simulation/)

## Example control files for analysis tools

- Structure analysis
  - [RMSD: root-mean-square deviation (`rmsd_analysis`)](/examples/rmsd_root-mean-square_deviation_rmsd_analysis/)
  - [Distance, Angle, Dihedral angle, COM distance, COM angle, COM dihedral angle (`trj_analysis`)](/examples/distance__angle__dihedral_angle__com_distance__com_angle__com_dihedral_angle_trj_analysis/)
  - [Radius of gyration (`rg_analysis`)](/examples/radius_of_gyration_rg_analysis/)
  - [Hydrogen bonds (`hb_analysis`)](/examples/hydrogen_bonds_hb_analysis/)
  - [Center of mass coordinates (`comcrd_analysis`)](/examples/center_of_mass_coordinates_comcrd_analysis/)
  - [Mean-square displacement (`msd_analysis`)](/examples/mean-square_displacement_msd_analysis/)
  - [Diffusion constant (`diffusion_analysis`)](/examples/diffusion_constant_diffusion_analysis/)
  - [Distance-distance matrix (`distmat_analysis`)](/examples/distance-distance_matrix_distmat_analysis/)
  - [RMSF: root-mean-square fluctuation (`flccrd_analysis`)](/examples/rmsf_root-mean-square_fluctuation_flccrd_analysis/)
  - [Membrane thickness (`lipidthick_analysis`)](/examples/membrane_thickness_lipidthick_analysis/)
  - [Tilt angle of transmebrane helix (`tilt_analysis`)](/examples/tilt_angle_of_transmebrane_helix_tilt_analysis/)
  - [Distance RMSD (`drms_analysis`)](/examples/distance_rmsd_drms_analysis/)
  - [Fraction of the native contacts (`qval_analysis`)](/examples/fraction_of_the_native_contacts_qval_analysis/)
  - [FRET efficiency (`fret_analysis`)](/examples/fret_efficiency_fret_analysis/)

- Spatial decomposition analysis (SPANA)
  - [General introduction of SPANA](/docs/spana/)
  - [Solvent accessible surface area (`sasa_analysis`)](/examples/solvent_accessible_surface_area_sasa_analysis/)
  - [Radial distribution function (`rdf_analysis`)](/examples/radial_distribution_function_rdf_analysis/)
  - [Density (`density_analysis`)](/examples/density_density_analysis/)
  - [Hydrogen bonds (`hbond_analysis`)](/examples/hydrogen_bonds_hbond_analysis/)
  - [Contact (`contact_analysis`)](/examples/contact_contact_analysis/)

- Principal component analysis
  - [Calculation of the averaged coordinates (`avecrd_analysis`)](/examples/calculation_of_the_averaged_coordinates_avecrd_analysis/)
  - [Calculation of the variance-covariance matrix (`flccrd_analysis`)](/examples/calculation_of_the_variance-covariance_matrix_flccrd_analysis/)
  - [Diagonalization of the variance-covariance matrix (`eigmat_analysis`)](/examples/diagonalization_of_the_variance-covariance_matrix_eigmat_analysis/)
  - [Projection of the coordinates trajectory onto PC axes (`prjcrd_analysis`)](/examples/projection_of_the_coordinates_trajectory_onto_pc_axes_prjcrd_analysis/)
  - [Visualization of the PC vectors by VMD or PyMol (pcavec_drawer)](/examples/visualization_of_the_pc_vectors_by_vmd_or_pymol_pcavec_drawer/)

- Free energy calculations
  - [WHAM: weighted histogram analysis method (`wham_analysis`)](/examples/wham_weighted_histogram_analysis_method_wham_analysis/)
  - [MBAR: multistate Bennett acceptance ratio (`mbar_analysis`)](/examples/mbar_multistate_bennett_acceptance_ratio_mbar_analysis/)
  - [Free energy profile using MBAR output (`pmf_analysis`)](/examples/free_energy_profile_using_mbar_output_pmf_analysis/)

- Interface program
  - [Protein secondary structure analysis by using the DSSP program (`dssp_interface`)](/examples/protein_secondary_structure_analysis_by_using_the_dssp_program_dssp_interface/)

- Clustering
  - [Structure clustering using the k-means algorithm (`kmeans_clustering`)](/examples/structure_clustering_using_the_k-means_algorithm_kmeans_clustering/)

- Converter
  - [Manipulate trajectories (combining, thinning, fitting, wrapping, centering, format change) (`crd_convert`)](/examples/manipulate_trajectories_combining__thinning__fitting__wrapping__centering__format_change_crd_convert/)
  - [Convert coordinates obtained from parallel I/O (`pcrd_convert`)](/examples/convert_coordinates_obtained_from_parallel_i_o_pcrd_convert/)
  - [Sort coordinates and energy trajectories by replica parameters (`remd_convert`)](/examples/sort_coordinates_and_energy_trajectories_by_replica_parameters_remd_convert/)
  - [Convert GENESIS restart file to PDB file (`rst_convert`)](/examples/convert_genesis_restart_file_to_pdb_file_rst_convert/)
  - [Convert restart file format from those in Ver. 1.0 to Ver. 1.1 or later (`rst_upgrade`)](/examples/convert_restart_file_format_from_those_in_ver__1_0_to_ver__1_1_or_later_rst_upgrade/)

- Other utilities
  - [Generate inputs for string method calculation from Targeted MD data (`rpath_generator`)](/examples/generate_inputs_for_string_method_calculation_from_targeted_md_data_rpath_generator/)
  - [Calculate tangential and orthogonal coordinates to a pathway from samples (`pathcv_analysis`)](/examples/calculate_tangential_and_orthogonal_coordinates_to_a_pathway_from_samples_pathcv_analysis/)
  - [Create synthetic EM density map from PDB file (`emmap_generator`)](/examples/create_synthetic_em_density_map_from_pdb_file_emmap_generator/)
  - [Generate a system for QM/MM calculation from MD data (`qmmm_generator`)](samples/qmmm_generator.html)

