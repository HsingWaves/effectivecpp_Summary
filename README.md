### This is the personel summary of effective c++
There are 55 terms how to write the "correct" c++ code in this book.

## Catalog 
* The fisrt 4 terms: Accustoming yourself to C++
  * [view c++ as a federation of languages](FirstChapter/View%20C++%20as%20federation%20language.md)
  * [Prefer const, enums, and inlines to #define](FirstChapter/Prefer%20const%20enum%20and%20inlines%20to%20%23define.md)
  * [Use const whenever possible](FirstChapter/Use%20const%20whenever%20possible.md)
  * [Make sure that objects are initilaed before they are used](FirstChapter/Make%20sure%20that%20objects%20are%20initialized%20before%20they%20are%20used.md)
* The seconde 5-12 terms: Constructors, deconstructor, and Assignment Operators
  * [Know what functions C++ sliently writes and calls](SecondChapter/WhatfunctionslientlyCall.md)
  * [Explicitly disallow the use of compiler-generated functions you do not want](SecondChapter/Explicitly_disallow.md)
  * [Declare destructors virtual in polymorphic base classes](SecondChapter/Declare_destructors_virtual.md)
  * [Preventing exceptions from leaving deconstructors](SecondChapter/Preventing_exceptions.md)
  * [never call virtual functions during construction and destruction](SecondChapter/Dont_call_virtual_InConstr.md)
  * [Have assignment operators return a reference to *this](SecondChapter/assignment_operator.md)
  * [Handle assignment to self in operator=](SecondChapter/self_assignment.md)
  * [Copy all parts of an object](SecondChapter/copy_all_parts.md)
* The third 13-17 terms: Memory management
  * [objects manage resources](ThirdChapter/Objmanage_resources.md)
  * [Think carefully about copying in resources managing classes](ThirdChapter/Think_carefull_copying.md)
  * [Provide access to raw resources](ThirdChapter/Provide_access_toraw.md)
  * [Use of the same form in corresponding new and delete](ThirdChapter/correspoding_uses_new_delete.md)
  * [smart pointers in standlone statements](ThirdChapter/store_newobj_smartpointer.md)
* The fourth 18-25 terms: Design and declarations
  * [Make interfaces easy to be used correctly](FourthChapter/make_interface_easy_tobe_used.md)
  * [Treat class design as type design](FourthChapter/Treat_class_design_as_type.md)
  * [Prefer pas-by-reference than pass-by-value](FourthChapter/pass_by_re_than_by_value.md)
  * [Don't try to return reference when an object must be returned](FourthChapter/dont_try_return_reference.md)
  * [Declare data members private](FourthChapter/member_data_as_private.md)
  * [Prefer non-member non-friend functions to member functions](FourthChapter/pre_nonmember_nonfriend_replacemember.md)
  * [Declare non-member functions when type conversion should apply to all parameters](FourthChapter/Declare_nonmember_functions_conversion_needed.md)
  * [Consider support for a non-throwing swap](FourthChapter/swap_function_no_throw.md)
* The fifth 26-31 chaptes: Implementation
  * [Postpone variable definitions as long as possible](FifthChapter/Postpone_variables_definition_aspossible.md)
  * [Minimize casting](FifthChapter/minimize_casting.md)
  * [Avoid returning "handles" to object internals](FifthChapter/avoid_handles_objects_internals.md)
  * [Strive for exceptions-safe code](FifthChapter/strive_for_exception_safe_code.md)
  * [Understand the ins and outs of inling](FifthChapter/understand_ins_outs_inlining.md)
  * [Minimize compilation dependencies between files](FifthChapter/minimize_compilation_dependencies_files.md)
* The sixth 32-40 terms: Inheritance and Object-Oriented Design
  * [Make sure public inheritance models "is-a"](SixthChapter/make_sure_inheritance_is_a.md)
  * [Avoid hiding inherited names](SixthChapter/Avoid_hiding_inherited_names.md)
  * [Differentiate between inheritance of interface and interface of implementation](SixthChapter/Diffentiate_inheritance_of_interface_or_of_implementation.md)
  * [Consider althernatives to virtual functions](SixthChapter/Consider_alternatives_to_virtualfuncitons.md)
  * [Never redefine an inherited non-virtual functions](SixthChapter/never_redefine_non_virtual_func.md)
  * [Never redefine a function's inherited default parameter value](SixthChapter/never_redefine_inherited_default_parameters.md)
  * [Model "has-a" or "is-implemented-in-terms-of" through composition](SixthChapter/Model_has_a_or_is_implemented_intermsof_through_composition.md)
  * [Use private inheritance judiciously](SixthChapter/Use_private_inheritance_judicicously.md)
  * [Use multiple inheritance judiciously](SixthChapter/use_mutiple_ineritance_judiciously.md)
* The seventh 41-48 terms: Templates and Generic Programming
  * [Understand implicit interfaces and compile-time polymorphism](SeventhChapter/Implicit_interface_compile_time_polymorphism.md)
  * [Understand two meanings of typename](SeventhChapter/two_meaning_typename.md)
  * [Know how to access names in templatized base classes](SeventhChapter/how_to_access_name_in_templatized_base_classes.md)
  * [Factor parameter-independent code out of templates](SeventhChapter/factor_paratameter_indepedent_code_out_template.md)
  * [Use member function templates to accept "all compatible types"](SeventhChapter/use_member_function_template_access_all_compatible_types.md)
  * [Define non-member functions inside templates when type conversion are desired](SeventhChapter/define_non_member_function_when_conversion.md)
  * [Use traits classes for information about types](SeventhChapter/traits_classes_for_information_about_types.md)
  * [Be aware of template metaprogramming](SeventhChapter/be_aware_template_metaprogramming.md)
* The eighth 49-52 terms: Customizing new and delete
  * [Understand the behavior of the new-handler](EighthChapter\new_handler_behavior.md)
  * [Understand when it makes sense to replace new and delete](EighthChapter\when_to_replace_new_delete.md)
  * [Adhere to convention when writing new and delete](EighthChapter\Adhere_to_convention_writing_new_delete.md)
  * [Write placement delete if you write placement new ](EighthChapter\write_placement_delete_if_writing_new.md)
* The last 53-55 terms: Miscellany
  * [Pay attention to compiler warnings](FinalChaper\pay_attention_to_warning.md)
  * [Familiarize yourself with the standard library, including TRI](FinalChaper\Familiarize_yourself_with_standarad_library.md)
  * [Familiarize yourself with Booost](FinalChaper\familiarize_with_Boost.md)
  
  
