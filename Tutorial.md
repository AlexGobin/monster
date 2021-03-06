# Tutorial
```cpp
#include <vector>
#include <iostream>
#include <forward_list>
#include <monster.hpp>

using namespace monster;

template <bool B>
void execute()
{
    static_assert(B);
}

template <auto p , auto q>
void execute()
{
    execute<p == q>();
}

template <typename T, typename U>
void execute()
{
    execute<std::is_same_v<T, U>>();
}

int main(int argc, char* argv[])
{
    execute<hanoi_t<1>, std::integer_sequence<int, 0, 1>>();
    execute<hanoi_t<2>, std::integer_sequence<int, 0, 2, 0, 1, 2, 1>>();
    execute<hanoi_t<3>, std::integer_sequence<int, 0, 1, 0, 2, 1, 2, 0, 1, 2, 0, 2, 1, 0, 1>>();

    execute<kmp_t<std::tuple<char>, std::tuple<int, float, double, int, int>>, std::index_sequence<>>();
    execute<kmp_t<std::tuple<double>, std::tuple<double, int, int, double>>, std::index_sequence<0, 3>>();

    execute<kmp_t<std::tuple<int, double, int, double>,
            std::tuple<int, int, double, int, double, double, char, int, double, int, double, int>>,
            std::index_sequence<1, 7>>();

    execute<kmp_t<std::integer_sequence<int, 1, 3, 1, 3, 1>,
            std::integer_sequence<int, -8, 2, -1, 1, 3, 1, 3, 1, 0, -4, 1, 3, 1, 3, 1, -5>>,
            std::index_sequence<3, 10>>();

    execute<kmp_t<std::integer_sequence<int, 2, 1, 3, 1, 3, 1>,
            std::integer_sequence<int, -8, 2, -1, 1, 3, 1, 3, 1, 0, -4, 1, 3, 1, 3, 1, -5>>,
            std::index_sequence<>>();

    std::cout << e<double> << std::endl; // 2.71828
    std::cout << pi<double> << std::endl; // 3.14159

    execute<int_<1>::value, 1>();
    execute<typev<int_<2>>, 2>();

    execute<value_t<std::true_type>, bool>();

    execute<!has_value_v<int>>();
    execute<negav<has_value<double>>>();
    execute<has_value_v<std::true_type>>();

    execute<first_t<pair_t<int, char>>, int>();
    execute<second_t<pair_t<float, double>>, double>();

    execute<first_v<pair_v<2, 4>>, 2>();
    execute<second_v<pair_v<3, 5>>, 5>();

    execute<identity_t<0, int>, int>();
    execute<wrapper_t<3, int>, std::type_identity<std::type_identity<std::type_identity<int>>>>();

    execute<last_t<int, double, char>, char>();
    execute<alias_t<int>, int>();

    execute<wrapin_t<std::type_identity, int, double>,
            std::tuple<std::type_identity<int>, std::type_identity<double>>>();

    execute<contains_v<int, double, char, float, int>>();
    execute<comprise_v<1, -2, 3, 0, 1>>();

    execute<is_unique_type_v<int, double, char, float>>();
    execute<has_duplicates_type_v<int, double, char, int, float>>();

    execute<!is_unique_value_v<1, 2, 2, 3, 4>>();
    execute<has_duplicates_value_v<1, 2, 3, 1, 4>>();

    execute<is_unique_v<std::tuple<int, double, char, float>>>();
    execute<is_unique_v<std::integer_sequence<int, 1, 2, 3, 4>>>();

    execute<has_duplicates_v<std::tuple<int, double, char, int, float>>>();
    execute<has_duplicates_v<std::integer_sequence<int, 1, 2, 3, 1, 4>>>();

    execute<enable_if_not_t<false, int>, int>();

    execute<is_streamable_v<std::ostream, int>>();
    execute<!is_streamable_v<std::ostream, std::tuple<int>>>();

    execute<!is_iterable_v<int>>();
    execute<is_iterable_v<std::vector<int>>>();

    execute<!is_container_v<int>>();
    execute<is_container_v<std::string>>();

    execute<is_pointer_of_v<std::add_pointer_t<int>, int>>();

    execute<!is_instance_of_v<std::tuple, int>>();
    execute<is_instance_of_v<std::tuple, std::tuple<int>>>();

    execute<!is_sequence_of_v<std::integer_sequence, int>>();
    execute<is_sequence_of_v<std::integer_sequence, std::index_sequence<1, 2, 3>>>();

    execute<sizeof_t_v<int_<8>>, 8>();
    execute<sizeof_t_v<int>, sizeof(int)>();

    execute<sizeof_t_v<std::tuple<int, char, double>>, 3>();
    execute<sizeof_t_v<std::index_sequence<1, 2, 0, 4>>, 4>();

    execute<argument_size_v<decltype([](int, char){})>, 2>();
    execute<argument_size_v<decltype(+[](int, int){})>, 2>();
    execute<argument_size_v<void(*)(int, char, short)>, 3>();

    execute<less_v<char, int>>();
    execute<less_equal_v<int_<3>, int_<4>>>();

    execute<equal_v<float, float>>();

    execute<greater_equal_v<double, float>>();
    execute<greater_v<double, int>>();

    execute<base_type_t<int>, int>();
    execute<base_type_t<std::tuple<int, char>>, std::tuple<>>();
    execute<base_type_t<std::integer_sequence<int, 1, 2, 4>>, std::integer_sequence<int>>();

    execute<homogeneous<0, 1, 2, 4>::size(), 4>();
    execute<sizeof_t_v<homogeneous<0, 1, 2, 4>>, 4>();

    execute<get_t<1, std::integer_sequence<int, 1, 2, 4>>, int_<2>>();
    execute<get_v<1, std::integer_sequence<int, 1, 2, 4>>, 2>();

    execute<sum_t<std::integer_sequence<int, 1, 2, 4>>, int_<7>>();
    execute<sum_v<std::integer_sequence<int, 1, 2, 4>>, 7>();

    execute<element_t<1, std::tuple<char, double, int>>, double>();
    execute<element_t<2, std::integer_sequence<int, 1, -2, 7, 4>>, int_<7>>();
    execute<element_v<3, std::integer_sequence<int, 1, -2, 7, 4>>, 4>();

    execute<front_t<std::tuple<char, double, int>>, char>();
    execute<front_t<std::integer_sequence<int, 1, -2, 7, 4>>, int_<1>>();
    execute<front_v<std::integer_sequence<int, 1, -2, 7, 4>>, 1>();

    execute<back_t<std::tuple<char, double, int>>, int>();
    execute<back_t<std::integer_sequence<int, 1, -2, 7, 4>>, int_<4>>();
    execute<back_v<std::integer_sequence<int, 1, -2, 7, 4>>, 4>();

    execute<plus_t<int_<3>, int_<4>>, int_<7>>();
    execute<plus_v<int_<3>, int_<4>>, 7>();

    execute<minus_t<int_<5>, int_<7>>, int_<-2>>();
    execute<minus_v<int_<5>, int_<7>>, -2>();

    execute<multiplies_t<int_<2>, int_<6>>, int_<12>>();
    execute<multiplies_v<int_<2>, int_<6>>, 12>();

    execute<divides_t<int_<11>, int_<5>>, int_<2>>();
    execute<divides_v<int_<11>, int_<5>>, 2>();

    execute<modulus_t<int_<12>, int_<7>>, int_<5>>();
    execute<modulus_v<int_<12>, int_<7>>, 5>();

    execute<negate_t<int_<-12>>, int_<12>>();
    execute<negate_v<int_<12>>, -12>();

    execute<pred_t<int_<3>>, int_<2>>();
    execute<pred_v<int_<3>>, 2>();

    execute<succ_t<int_<3>>, int_<4>>();
    execute<succ_v<int_<3>>, 4>();

    execute<prev_t<2, std::tuple<int, double, char, float, char>>, double>();
    execute<prev_t<1, std::integer_sequence<int, 7, 3, -1, 2, 4>>, int_<7>>();
    execute<prev_v<1, std::integer_sequence<int, 7, 3, -1, 2, 4>>, 7>();

    execute<next_t<2, std::tuple<int, double, char, float, char>>, float>();
    execute<next_t<1, std::integer_sequence<int, 7, 3, -1, 2, 4>>, int_<-1>>();
    execute<next_v<1, std::integer_sequence<int, 7, 3, -1, 2, 4>>, -1>();

    execute<gcd_v<4, 4>, 4>();
    execute<gcd_v<4, 18>, 2>();
    execute<gcd_v<7, 13>, 1>();

    execute<lcm_v<4, 4>, 4>();
    execute<lcm_v<4, 18>, 36>();
    execute<lcm_v<7, 13>, 91>();

    execute<unpack_t<max_type_t, std::tuple<int, double, char>>, max_type_t<int, double, char>>();

    using bf = bind_front<max_type_t, int, double>;
    execute<bf::template apply<char, float>, max_type_t<int, double, char, float>>();
    execute<call_t<bf, char, float>, max_type_t<int, double, char, float>>();
    execute<call_t<partial<bf, char>, float>, max_type_t<int, double, char, float>>();

    using bb = bind_back<std::tuple, int, double>;
    execute<bb::template apply<char, float>, std::tuple<char, float, int, double>>();
    execute<call_t<bb, char, float>, std::tuple<char, float, int, double>>();
    execute<call_t<partial<bb, char>, float>, std::tuple<char, float, int, double>>();

    execute<call_t<bind_front<std::add_pointer>, int*>, int**>();

    execute<folded_t<max, int, char, double>, double>();
    execute<folded_t<min, int, char, double>, char>();

    execute<curried_t<std::add_pointer, std::tuple, int*, double>, std::tuple<int**, double*>>();
    execute<currying_t<std::add_pointer, int*, double, char>, std::tuple<int**, double*, char*>>();
    execute<curry_t<recurse<std::add_pointer, std::tuple>, int*, double, short>,
            std::tuple<int**, double*, short*>>();

    execute<negav<eval_t<folded_t, std::conjunction, std::tuple<std::false_type, std::true_type>,
            std::true_type>>>();

    execute<on_t<std::add_pointer, std::tuple, int, double, char*>, std::tuple<int*, double*, char**>>();

    execute<equal<3>::template apply<int_<3>>::value>();

    execute<first_of_v<is_tuple, std::tuple<int, char, std::tuple<double>, float>>, 2>();
    execute<first_not_of_v<is_tuple, std::tuple<std::tuple<int>, char, std::tuple<double>, float>>, 1>();

    execute<last_of_v<equal<2>::template apply, std::integer_sequence<int, 1, 2, 3, 2, 4, 1, 2>>, 6>();
    execute<last_not_of_v<equal<2>::template apply, std::integer_sequence<int, 1, 2, 3, 2, 4, 1, 2>>, 5>();

    using lists = std::integer_sequence<int, 1, 1, 2, 3, 3, 3, 3, 4, 4, 4, 5, 5, 6>;
    using equal_range = equal_range_t<int_<4>, lists>;
    execute<range_t<equal_range::first, equal_range::second, lists>, std::integer_sequence<int, 4, 4, 4>>();

    execute<any_of_v<is_odd, std::integer_sequence<int, 1, 3, 2, 7, 8, 0, 1, -2, 4, 5>, 0, 10>>();
    execute<any_of_v<is_tuple, std::tuple<int, char, std::tuple<char>, double,
            std::tuple<uint64_t>, int, std::tuple<float>>, 0, 7>>();

    execute<adjacent_difference_t<minus_t, std::integer_sequence<int, 2, 4, 6, 8>>,
            std::integer_sequence<int, 2, 2, 2, 2>>();

    execute<adjacent_find_v<greater_t, std::integer_sequence<int, 0, 1, 2, 2, 4, 4, 3>>, 5>();
    execute<adjacent_find_v<std::is_same, std::integer_sequence<int, 0, 1, 2, 2, 4, 4, 3>>, 2>();

    execute<invert_type_t<>, invert_type<>>();
    execute<invert_type_t<char>, char>();
    execute<invert_type_t<char, double>, invert_type<double, char>>();
    execute<invert_type_t<int, char, double, float>, invert_type<float, double, char, int>>();

    execute<invert_value_t<>, invert_value<>>();
    execute<invert_value_t<1>, invert_value<1>>();
    execute<invert_value_t<1, 2>, invert_value<2, 1>>();
    execute<invert_value_t<2, 1, 4, 3>, invert_value<3, 4, 1, 2>>();

    execute<flip_t<std::tuple, int, char, double>, std::tuple<double, char, int>>();

    execute<set_difference_t<less_t, std::integer_sequence<int, 1, 2, 5, 5, 5, 9>,
            std::integer_sequence<int, 2, 5, 7>>, std::integer_sequence<int, 1, 5, 5, 9>>();

    execute<set_symmetric_difference_t<less_t, std::integer_sequence<int, 1, 2, 5, 5, 5, 9>,
            std::integer_sequence<int, 2, 5, 7>>, std::integer_sequence<int, 1, 5, 5, 7, 9>>();

    execute<set_intersection_t<less_t, std::integer_sequence<int, 1, 2, 3, 4, 5, 6, 7, 8>,
            std::integer_sequence<int, 5, 7, 9, 10>>, std::integer_sequence<int, 5, 7>>();

    execute<set_union_t<less_t, std::integer_sequence<int, 1, 2, 3, 4, 5, 5, 5>,
            std::integer_sequence<int, 3, 4, 5, 6, 7>>,
            std::integer_sequence<int, 1, 2, 3, 4, 5, 5, 5, 6, 7>>();

    execute<mismatch_t<equal_t, std::integer_sequence<int, 1, 2, 3, 4, 5, 5, 5>,
            std::integer_sequence<int, 1, 2, 3, 5, 6, 7>>, pair_v<3, 3>>();

    execute<includes_v<equal_t, std::integer_sequence<int, 1, 2, 3, 4, 5, 7>,
            std::integer_sequence<int, 2, 4, 7>>>();

    execute<search_v<equal_t, std::integer_sequence<int, 1, 2, 3, 4, 5, 7>,
            std::integer_sequence<int, 3, 4, 5>>, 2>();
    execute<find_end_v<equal_t, std::integer_sequence<int, 0, 1, 2, 0, 1, 2, 0, 1, 2>,
            std::integer_sequence<int, 0, 1, 2>>, 6>();

    execute<search_n_v<std::is_same, 2, int_<2>, std::integer_sequence<int, 1, 2, 0, 2, 2, 7, 2>>, 3>();
    execute<search_n_v<std::is_same, 2, int, std::tuple<int, char, int, int, double, int, int, char>>, 2>();

    execute<partial_sum_first<plus_t, std::integer_sequence<int, 1, 2, 3, 4>>, int_<10>>();
    execute<partial_sum_second<plus_t, std::integer_sequence<int, 1, 2, 3, 4>>,
            std::integer_sequence<int, 1, 3, 6, 10>>();

    execute<transmute_t<plus_t, std::integer_sequence<int, 1, 3, 0, 2>,
            std::integer_sequence<int, 1, 5, 4, 2>>, std::integer_sequence<int, 2, 8, 4, 4>>();

    execute<inner_product_v<plus_t, equal_t, std::integer_sequence<int, 1, 3, 0, 2>,
            std::integer_sequence<int, 1, 5, 4, 2>, int_<0>>, 2>();
    execute<inner_product_v<plus_t, multiplies_t, std::integer_sequence<int, 1, 3, 0, 2>,
            std::integer_sequence<int, 1, 5, 4, 2>, int_<0>>, 20>();

    execute<accumulate_v<multiplies_t, int_<1>, std::integer_sequence<int, 1, 3, -3, 2>>, -18>();

    execute<iota_t<succ, int_<-2>, std::integer_sequence<int, 0, 0, 0, 0, 0>>,
            std::integer_sequence<int, -2, -1, 0, 1, 2>>();

    using nested = std::tuple<std::tuple<char, short>, double,
                   std::tuple<char, std::tuple<char, int, double>>, short>;
    using reversed = reverse_recursive_t<nested>;
    execute<reversed, reverse_t<std::tuple<reverse_t<std::tuple<char, short>>, double,
            reverse_t<std::tuple<char, reverse_t<std::tuple<char, int, double>>>>, short>>>();

    using integer_t = std::integer_sequence<int, 2, 7, 4>;
    execute<increase_t<integer_t, 3>, std::integer_sequence<int, 5, 10, 7>>();
    execute<increase_t<integer_t, -3>, std::integer_sequence<int, -1, 4, 1>>();

    using add_one_t = eval_t<currying_t, succ, to_tuple_t<integer_t>>;
    execute<add_one_t, std::tuple<int_<3, int>, int_<8, int>, int_<5, int>>>();

    execute<next_slide_indices_t<2, 7, std::integer_sequence<int, 2, 3, 4>>,
            std::integer_sequence<int, 3, 4, 5>>();
    execute<prev_slide_indices_t<2, 7, std::integer_sequence<int, 3, 4, 5>>,
            std::integer_sequence<int, 2, 3, 4>>();

    using indices = std::tuple<std::integer_sequence<int, 2, 3, 4>,
                               std::integer_sequence<int, 3, 4, 5>,
                               std::integer_sequence<int, 4, 5, 6>>;

    execute<next_slide_indices_list<2, 7, std::integer_sequence<int, 2, 3, 4>>, indices>();
    execute<prev_slide_indices_list<2, 7, std::integer_sequence<int, 4, 5, 6>>, reverse_t<indices>>();

    execute<slide_list_t<3, std::integer_sequence<int, 1, 2, 3, 4>>,
            std::tuple<std::integer_sequence<int, 1, 2, 3>, std::integer_sequence<int, 2, 3, 4>>>();

    execute<slide_list_t<3, std::tuple<int, char, double, float>>,
            std::tuple<std::tuple<int, char, double>, std::tuple<char, double, float>>>();

    execute<nth_type_t<2, int, double, char*, float>, char*>();
    execute<first_type<int, double, char*, float>, int>();
    execute<last_type<int, double, char*, float>, float>();

    execute<arg_t<arg<1>, int, float, double>, float>();

    execute<make_index_tuple<4>, index_tuple<0, 1, 2, 3>>();
    execute<index_tuple_for<int, char, double>, index_tuple<0, 1, 2>>();

    execute<make_index_sequence<4>, index_sequence<0, 1, 2, 3>>();
    execute<index_sequence_for<int, char, double>, index_sequence<0, 1, 2>>();

    execute<range_generator_t<size_t, 1, 4>, index_list<size_t, 1, 2, 3>>();
    execute<index_range<2, 5>, index_list<size_t, 2, 3, 4>>();

    fmap([](auto& v)
    {
        std::cout << v << std::endl;
    }, 1, 2.3, "string", 'x');

    execute<concat_t<std::tuple<int, char>, std::tuple<double>>, std::tuple<int, char, double>>();
    execute<concat_t<std::integer_sequence<int, 1, 2, 3>, std::integer_sequence<int, 4, 5, 6>>,
            std::integer_sequence<int, 1, 2, 3, 4, 5, 6>>();

    execute<unique_t<std::tuple<int, char, int, double>> , std::tuple<char, int, double>>();
    execute<unique_t<std::integer_sequence<int, 2, 2, 3, 4, 3>>, std::integer_sequence<int, 2, 4, 3>>();

    execute<unique_if_t<is_pointer_of, std::tuple<int*, int*, int, double, char, double*, double, float>>,
            std::tuple<int*, int*, double, char, double*, float>>();

    using value = std::integer_sequence<int, 4, 0, 5, 6, 4, 1, 9, 21>;
    using type = std::tuple<int, char, std::tuple<char>, double, std::tuple<double>, float, char>;

    execute<is_even_v<int_<4>>>();
    execute<!call<negaf<is_even>, int_<4>>::value>();
    execute<!negaf<is_even>::template apply<int_<4>>::value>();

    execute<remove_if_t<is_odd, value, 1, 6>, std::integer_sequence<int, 4, 0, 6, 4, 9, 21>>();
    execute<remove_if_t<is_even, value, 1, 6>, std::integer_sequence<int, 4, 5, 1, 9, 21>>();
    execute<remove_if_t<is_tuple, type, 1, 5>, std::tuple<int, char, double, float, char>>();
    execute<remove_if_t<negaf<is_tuple>::template apply, type, 1, 5>,
            std::tuple<int, std::tuple<char>, std::tuple<double>, float, char>>();

    execute<remove_t<is_tuple, type>, std::tuple<int, char, double, float, char>>();
    execute<remove_t<negaf<is_tuple>::template apply, type>,
            std::tuple<std::tuple<char>, std::tuple<double>>>();

    execute<remove_t<is_odd, std::integer_sequence<int, 0, 3, 2, 4, 7, 3>>,
            std::integer_sequence<int, 0, 2, 4>>();
    execute<remove_t<is_even, std::integer_sequence<int, 0, 3, 2, 4, 7, 3>>,
            std::integer_sequence<int, 3, 7, 3>>();

    execute<eliminate_t<char, std::tuple<int, char, double, float, char>>,
            std::tuple<int, double, float>>();
    execute<eliminate_t<int_<3>, std::integer_sequence<int, 0, 3, 2, 4, 7, 3>>,
            std::integer_sequence<int, 0, 2, 4, 7>>();
    execute<eliminate_c<4, std::integer_sequence<int, 4, 3, 2, 4, 7, 3>>,
            std::integer_sequence<int, 3, 2, 7, 3>>();

    execute<maximum_t<std::tuple<short, int, char>>, int>();
    execute<minimum_t<std::tuple<short, int, char>>, char>();
    execute<maximum_t<std::integer_sequence<int, 1, -2, 3, 0, 2, 4>>, int_<4>>();
    execute<minimum_t<std::integer_sequence<int, 1, -2, 3, 0, 2, 4>>, int_<-2>>();
    execute<maximum_v<std::integer_sequence<int, 1, -2, 3, 0, 2, 4>>, 4>();
    execute<minimum_v<std::integer_sequence<int, 1, -2, 3, 0, 2, 4>>, -2>();

    execute<min_t<char, double>, char>();
    execute<max_t<char, double>, double>();

    execute<min_type_t<short, int, char>, char>();
    execute<max_type_t<short, int, char>, int>();

    execute<expand_t<std::tuple_element, std::tuple<int, float, char, double>,
            std::index_sequence<0, 2>>, std::tuple<int, char>>();
    execute<expand_t<get, std::integer_sequence<int, -1, 3, 7, 0, 5>,
            std::index_sequence<1, 3, 0, 4>>, std::integer_sequence<int, 3, 0, -1, 5>>();

    execute<expand_t<element, std::tuple<int, float, char, double>,
            std::index_sequence<0, 2>>, std::tuple<int, char>>();
    execute<expand_t<element, std::integer_sequence<int, -1, 3, 7, 0, 5>,
            std::index_sequence<1, 3, 0, 4>>, std::integer_sequence<int, 3, 0, -1, 5>>();

    execute<expand_of_t<std::tuple<int, float, char, double>,
            std::index_sequence<0, 2>>, std::tuple<int, char>>();
    execute<expand_of_t<std::integer_sequence<int, -1, 3, 7, 0, 5>,
            std::index_sequence<1, 3, 0, 4>>, std::integer_sequence<int, 3, 0, -1, 5>>();

    execute<to_sequence_t<std::tuple<int, double>>, std::integer_sequence<int, 4, 8>>();
    execute<to_sequence_t<std::index_sequence<4, 8>>, std::index_sequence<4, 8>>();

    execute<to_tuple_t<std::integer_sequence<int, 4, 8>>, std::tuple<int_<4>, int_<8>>>();
    execute<to_tuple_t<std::tuple<int, double>>, std::tuple<int, double>>();

    execute<swap_t<1, 3, std::tuple<int, double, char, float>>, std::tuple<int, float, char, double>>();
    execute<swap_t<0, 2, std::integer_sequence<int, 1, -2, 7, 4>>,
            std::integer_sequence<int, 7, -2, 1, 4>>();

    using ranges_t = swap_ranges<1, 4, std::integer_sequence<int, 0, 1, 2, 3, 4, 5>,
          std::integer_sequence<int, -1, -2, -3, -4, -5>>;

    execute<first_t<ranges_t>, std::integer_sequence<int, 0, -2, -3, -4, 4, 5>>();
    execute<second_t<ranges_t>, std::integer_sequence<int, -1, 1, 2, 3, -5>>();

    execute<swap_ranges_first<1, 4, std::integer_sequence<int, 0, 1, 2, 3, 4, 5>,
            std::integer_sequence<int, -1, -2, -3, -4, -5>>,
            std::integer_sequence<int, 0, -2, -3, -4, 4, 5>>();

    execute<swap_ranges_second<0, 3, std::integer_sequence<int, 1, 2, 3, 4>,
            std::integer_sequence<int, -1, -2, -3, -4, -5>>,
            std::integer_sequence<int, 1, 2, 3, -4, -5>>();

    using extents_t = swap_extent<1, 4, 2, 5, std::integer_sequence<int, 0, 1, 2, 3, 4, 5>,
          std::integer_sequence<int, -1, -2, -3, -4, -5>>;

    execute<first_t<extents_t>, std::integer_sequence<int, 0, -3, -4, -5, 4, 5>>();
    execute<second_t<extents_t>, std::integer_sequence<int, -1, -2, 1, 2, 3>>();

    execute<swap_extent_first<1, 4, 2, 5, std::integer_sequence<int, 0, 1, 2, 3, 4, 5>,
            std::integer_sequence<int, -1, -2, -3, -4, -5>>,
            std::integer_sequence<int, 0, -3, -4, -5, 4, 5>>();

    execute<swap_extent_second<0, 3, 1, 4, std::integer_sequence<int, 1, 2, 3, 4>,
            std::integer_sequence<int, -1, -2, -3, -4, -5>>,
            std::integer_sequence<int, -1, 1, 2, 3, -5>>();

    execute<within_v<2, -2, 4>>();
    execute<!within_v<5, -2, 4>>();

    execute<gsub_t<1, 5, int, float, std::tuple<int, char, int, double, int, float, char>>,
            std::tuple<int, char, float, double, float, float, char>>();
    execute<gsub_t<1, 5, int_<-2>, int_<-6>, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, 1, 3, -6, 1, 7, 1, 4, 6>>();
    execute<gsub_c<1, 5, -2, -6, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, 1, 3, -6, 1, 7, 1, 4, 6>>();

    execute<replace_with_t<int, float, std::tuple<int, char, int, double, int, float, char>>,
            std::tuple<float, char, float, double, float, float, char>>();
    execute<replace_with_t<int_<1>, int_<-6>, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, -6, 3, -2, -6, 7, -6, 4, 6>>();
    execute<replace_with_c<1, -6, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, -6, 3, -2, -6, 7, -6, 4, 6>>();

    execute<gsub_if_t<1, 5, is_tuple, int, std::tuple<int, char, std::tuple<>, double,
            std::tuple<>, float, char>>, std::tuple<int, char, int, double, int, float, char>>();
    execute<gsub_if_t<1, 5, is_even, int_<-6>, std::integer_sequence<int, 1, 3, -2, 4, 7, 0, 4, 6>>,
            std::integer_sequence<int, 1, 3, -6, -6, 7, 0, 4, 6>>();

    execute<adjust_t<1, 5, int, std::tuple<int, char, int, double, int, float, char>, std::add_pointer>,
            std::tuple<int, char, int*, double, int*, float, char>>();
    execute<adjust_t<1, 5, int_<1>, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>, succ>,
            std::integer_sequence<int, 1, 3, -2, 2, 7, 1, 4, 6>>();

    execute<adjust_if_t<1, 5, std::tuple<int, std::tuple<int>, double,
            std::tuple<float>, char>, std::add_pointer, is_tuple>,
            std::tuple<int, std::tuple<int>*, double, std::tuple<float>*, char>>();
    execute<adjust_if_t<1, 5, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>, succ, is_odd>,
            std::integer_sequence<int, 1, 4, -2, 2, 8, 1, 4, 6>>();

    execute<substitute_t<2, 5, int, std::tuple<int, char, int, double, int, float, char>>,
            std::tuple<int, char, int, int, int, float, char>>();
    execute<substitute_t<2, 5, int_<-2>, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, 1, 3, -2, -2, -2, 1, 4, 6>>();
    execute<substitute_c<3, 6, 9, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, 1, 3, -2, 9, 9, 9, 4, 6>>();

    execute<exchange_t<2, int, std::tuple<int, int, char, double, int, float, char>>,
            std::tuple<int, int, int, double, int, float, char>>();
    execute<exchange_t<1, int_<-2>, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, 1, -2, -2, 1, 7, 1, 4, 6>>();
    execute<exchange_c<4, 9, std::integer_sequence<int, 1, 3, -2, 1, 7, 1, 4, 6>>,
            std::integer_sequence<int, 1, 3, -2, 1, 9, 1, 4, 6>>();

    execute<sub_t<3, 8, range_generator_t<int, -2, 3>>, index_list<int, -2, -1, 0, 8, 2>>();
    execute<sub_t<2, 3, index_list<size_t, 3, 4, 0, 5, 2>>, index_list<size_t, 3, 4, 3, 5, 2>>();
    execute<sub_t<4, -3, std::integer_sequence<int, -1, 8, 0, 2, 4, 7>>,
            std::integer_sequence<int, -1, 8, 0, 2, -3, 7>>();

    execute<range_t<1, 4, int>, int>();
    execute<range_t<1, 4, std::tuple<int, char, float, double, int>>, std::tuple<char, float, double>>();
    execute<range_t<1, 4, std::integer_sequence<int, 1, 2, -2, 4, 3, 5>>,
            std::integer_sequence<int, 2, -2, 4>>();

    execute<reverse_t<double>, double>();
    execute<reverse_t<std::tuple<float, double, int>>, std::tuple<int, double, float>>();
    execute<reverse_t<std::integer_sequence<int, 1, 2, -2>>, std::integer_sequence<int, -2, 2, 1>>();

    execute<extent_t<1, 4, char>, char>();
    execute<extent_t<1, 4, std::tuple<int, char, float, double, int, char>>,
            std::tuple<double, float, char>>();
    execute<extent_t<1, 4, std::integer_sequence<int, 1, 2, -2, 4, 3, 5>>,
            std::integer_sequence<int, 4, -2, 2>>();

    execute<erase_t<2, 5, std::tuple<int, char, float, double, int, char>>,
            std::tuple<int, char, char>>();
    execute<erase_t<1, 4, std::integer_sequence<int, 1, 2, -2, 4, 3, 5>>,
            std::integer_sequence<int, 1, 3, 5>>();

    execute<drop_t<2, std::tuple<int, double, int, char>>, std::tuple<int, double, char>>();
    execute<drop_t<1, std::integer_sequence<int, 1, 2, -2, 4, 5>>,
            std::integer_sequence<int, 1, -2, 4, 5>>();

    execute<drop_front_t<2, std::tuple<int, double, int, char>>, std::tuple<int, char>>();
    execute<drop_back_t<2, std::integer_sequence<int, 1, 2, -2, 4, 5>>,
            std::integer_sequence<int, 1, 2, -2>>();

    execute<take_front_t<2, std::tuple<int, double, int, char>>, std::tuple<int, double>>();
    execute<take_back_t<2, std::integer_sequence<int, 1, 2, -2, 4, 5>>,
            std::integer_sequence<int, 4, 5>>();

    execute<degree_t<1, 4, std::tuple<int, char, float, double, int, char>>,
            std::tuple<int, double, float>>();
    execute<degree_t<1, 4, std::integer_sequence<int, 1, 2, -2, 4, 3, 5>>,
            std::integer_sequence<int, 3, 4, -2>>();

    execute<reverse_range_t<1, 4, float>, float>();
    execute<reverse_range_t<1, 4, std::tuple<int, char, float, double, int, char>>,
            std::tuple<int, double, float, char, int, char>>();
    execute<reverse_range_t<1, 4, std::integer_sequence<int, 1, 2, -2, 4, 3, 5>>,
            std::integer_sequence<int, 1, 4, -2, 2, 3, 5>>();

    execute<typeindex<char, float, double, int, char>(), 3>();

    execute<type_index_v<int, std::tuple<char, float, double, int, char>>, 3>();
    execute<type_index_v<uint64_t, std::tuple<char, float, double, int, char>>, 5>();
    execute<value_index_v<6, std::integer_sequence<int, 1, -2, 0, 3, 6, 5>>, 4>();
    execute<value_index_v<7, std::integer_sequence<int, 1, -2, 0, 3, 6, 5>>, 6>();

    execute<tuple_element_size_v<2, std::tuple<short, int, double>>, sizeof(double)>();

    execute<binary_search_v<double, std::tuple<short, int, double>>>();
    execute<binary_search_v<int_<7>, std::integer_sequence<int, -2, 0, 3, 7, 8>>>();

    execute<exponential_search_v<double, std::tuple<short, int, double>>>();
    execute<exponential_search_v<int_<7>, std::integer_sequence<int, -2, 0, 3, 7, 8>>>();

    execute<interpolation_search_v<double, std::tuple<short, int, double>>>();
    execute<interpolation_search_v<int_<7>, std::integer_sequence<int, -2, 0, 3, 7, 8>>>();

    execute<fibonacci_search_v<double, std::tuple<short, int, double>>>();
    execute<fibonacci_search_v<int_<7>, std::integer_sequence<int, -2, 0, 3, 7, 8>>>();

    execute<majority_search_v<std::integer_sequence<int, 1, 2, 2, 3, 2, 1, 2>>, 2>();
    execute<majority_search_t<std::tuple<char, int, int, float, int, double, int>>, int>();

    execute<max_subarray_t<std::integer_sequence<int, -2, -3, 4, -1, -2, 1, 5, -3>>,
            std::integer_sequence<int, 2, 6, 7>>();

    execute<find_maximum_subarray_t<std::integer_sequence<int, -2, -3, 4, -1, -2, 1, 5, -3>>,
            std::integer_sequence<int, 2, 6, 7>>();

    execute<kadane_t<std::integer_sequence<int, -2, -3, 4, -1, -2, 1, 5, -3>>, int_<7>>();
    execute<kadane_v<std::integer_sequence<int, -2, -3, 4, -1, -2, 1, 5, -3>>, 7>();

    execute<inner_dot_t<std::integer_sequence<int, -3, 1, 2, 4, 5>,
            std::integer_sequence<int, 2, 4, -3, 4>>, std::integer_sequence<int, -6, 4, -6, 16>>();

    execute<alter_t<std::integer_sequence<int, 1, 0, 3, 4>, size_t>,
            std::integer_sequence<size_t, 1, 0, 3, 4>>();

    execute<splat_t<2, 4, std::integer_sequence<int, 1, 0, 3, 4>>,
            std::integer_sequence<int, 3, 3, 3, 3>>();

    execute<splat_t<3, 4, std::tuple<int, char, short, int, double, char>>,
            std::tuple<int, int, int, int>>();

    overload_sequence element_count
    {
        [](auto&&) { return 1; },
        [](auto&& v) -> decltype(v.size())
        {
             return std::forward<decltype(v)>(v).size();
        },
        [](auto&& v) -> type_t<std::size_t, decltype(std::begin(v)), decltype(std::end(v))>
        {
            return std::distance(std::begin(std::forward<decltype(v)>(v)),
                   std::end(std::forward<decltype(v)>(v)));
        }
    };

    element_count(42);
    element_count(std::vector({1 , 2, 3, 4}));
    element_count(std::forward_list({1 , 2, 3}));

    overload_set
    {
        [](auto x) { return x; },
        [](int x) { return x * 10; },
        [](double x) { return x / 10.; }
    }(10.0);

    for_value<1, -2, 3>([]<auto N>()
    {
        std::cout<< N << std::endl;
    });

    for_range<4, 7>([]<auto N>()
    {
        std::cout<< N << std::endl;
    });

    for_type<double, int, char>([]<auto N, typename T>()
    {
        std::cout << N << " " << typeid(T).name() << std::endl;
    });

    loop<3>([](auto N, auto&& v)
    {
        std::cout << N << " " << v << std::endl;
    }, 4);

    loop_for<1>(2, 3, 4)
    (
        [](auto i, auto j, auto k)
        {
            std::cout << '(' << i << ", " << j << ", " << k << ")" << std::endl;
        }
    );

    std::cout << std::endl;

    loop_for<0>(2, 3, 4)
    (
        [](auto i, auto j, auto k)
        {
            std::cout << '(' << i << ", " << j << ", " << k << ")" << std::endl;
        }
    );

    iterate<4>([]
    {
        std::cout << "x" << std::endl;
    });

    unroller<3> unroll;

    unroll([](auto&&... args)
    {
        (std::cout << ... << args) << std::endl;
    }, 1, 2.3, "[]<typename ...>(){}();");

    auto tuple = std::make_tuple(1, 2, 3.0, 2, 4);
    std::cout << "tuple_apply " << tuple_apply([](auto... args){ return (args + ...); }, tuple) << std::endl;
    std::cout << "tuple_reverse " << std::get<0>(tuple_reverse(tuple)) << std::endl;
    std::cout << "tuple_prepend " << std::get<0>(tuple_prepend(tuple, 0)) << std::endl;
    std::cout << "tuple_append " << std::get<5>(tuple_append(tuple, 8)) << std::endl;
    std::cout << "tuple_remove " << std::get<1>(tuple_remove<1>(tuple)) << std::endl;
    std::cout << "tuple_swap " << std::get<1>(tuple_swap<1, 4>(tuple)) << std::endl;
    std::cout << "tuple_range " << std::get<1>(tuple_range<1, 4>(tuple)) << std::endl;
    std::cout << "tuple_erase " << std::get<1>(tuple_erase<1, 4>(tuple)) << std::endl;
    std::cout << "tuple_reverse_range " << std::get<2>(tuple_reverse_range<2, 5>(tuple)) << std::endl;
    std::cout << "tuple_insert " << std::get<2>(tuple_insert<2>(tuple, 9)) << std::endl;
    std::cout << "tuple_replace " << std::get<3>(tuple_replace<3>(tuple, 7)) << std::endl;
    std::cout << "tuple_replace all " << std::get<1>(tuple_replace(tuple, 2, 6)) << std::endl;
    std::cout << "tuple_replace all " << std::get<3>(tuple_replace(tuple, 2, 6)) << std::endl;
    std::cout << "tuple_rotate " << std::get<2>(tuple_rotate<0, 2, 4>(tuple)) << std::endl;
    std::cout << "tuple_shift_left " << std::get<1>(tuple_shift_left<2>(tuple)) << std::endl;
    std::cout << "tuple_shift_right " << std::get<1>(tuple_shift_right<2>(tuple)) << std::endl;
    std::cout << "tuple_take_front " << std::get<1>(tuple_take_front<3>(tuple)) << std::endl;
    std::cout << "tuple_take_back " << std::get<2>(tuple_take_back<3>(tuple)) << std::endl;
    std::cout << "tuple_drop_front " << std::get<0>(tuple_drop_front<3>(tuple)) << std::endl;
    std::cout << "tuple_drop_back " << std::get<1>(tuple_drop_back<3>(tuple)) << std::endl;
    std::cout << "tuple_zip " << std::get<1>(tuple_zip(tuple, std::make_tuple(7, 5))) << std::endl;
    tuple_fill<3>(tuple, 3, 5);
    std::cout << "tuple_fill " << std::get<4>(tuple) << std::endl;
    std::cout << "tuple_splat " << std::get<2>(tuple_splat<2, 5>(tuple)) << std::endl;

    execute<infinite_call_t<4, int, std::add_pointer>, int****>();
    execute<multi_level_pointer_t<4, int>, int****>();

    execute<pop_front_t<std::tuple<float, double, int>>, std::tuple<double, int>>();
    execute<pop_front_t<std::integer_sequence<int, 1, 2, 3>>, std::integer_sequence<int, 2, 3>>();
    execute<pop_back_t<std::tuple<float, double, int>>, std::tuple<float, double>>();
    execute<pop_back_t<std::integer_sequence<int, 1, 2, 3>>, std::integer_sequence<int, 1, 2>>();

    execute<prepend_t<std::tuple<float, double, int>, int, float>,
            std::tuple<int, float, float, double, int>>();
    execute<prepend_t<std::integer_sequence<int, 1, 2, -2>, int_<4>, int_<3>>,
            std::integer_sequence<int, 4, 3, 1, 2, -2>>();
    execute<prepend_c<std::integer_sequence<int, 1, 2, -2>, 4, 3>,
            std::integer_sequence<int, 4, 3, 1, 2, -2>>();

    execute<append_t<std::tuple<float, double, int>, float, char>,
            std::tuple<float, double, int, float, char>>();
    execute<append_t<std::integer_sequence<int, 1, 2, -2>, int_<4>, int_<3>>,
            std::integer_sequence<int, 1, 2, -2, 4, 3>>();
    execute<append_c<std::integer_sequence<int, 1, 2, -2>, 4, 3>,
            std::integer_sequence<int, 1, 2, -2, 4, 3>>();

    execute<replace_t<1, 3, std::tuple<char, float, double, int>, float, char>,
            std::tuple<char, float, char, int>>();
    execute<replace_c<1, 3, std::integer_sequence<int, 4, 1, 2, -2>, 4, 3>,
            std::integer_sequence<int, 4, 4, 3, -2>>();

    execute<replace_range_t<1, 3, std::tuple<char, float, double, int>,
            std::tuple<float, char, int, double>, 1, 3>, std::tuple<char, char, int, int>>();
    execute<replace_range_t<1, 3, std::integer_sequence<int, 4, 1, 2, -2>,
            std::integer_sequence<int, -2, 1, 0, 3>, 0, 3>,
            std::integer_sequence<int, 4, -2, 1, 0, -2>>();

    execute<insert_t<2, std::tuple<float, double, int>, float, char>,
            std::tuple<float, double, float, char, int>>();
    execute<insert_t<1, std::integer_sequence<int, 1, 2, -2>, int_<0>, int_<4>>,
            std::integer_sequence<int, 1, 0, 4, 2, -2>>();
    execute<insert_c<1, std::integer_sequence<int, 1, 2, -2>, 0, 4>,
            std::integer_sequence<int, 1, 0, 4, 2, -2>>();

    execute<insert_range_t<2, std::tuple<float, double, int>, std::tuple<float, char, char>, 0, 2>,
            std::tuple<float, double, float, char, int>>();
    execute<insert_range_t<1, std::integer_sequence<int, 1, 2, -2>,
            std::integer_sequence<int, 0, 9, -4, 3>, 1, 3>, std::integer_sequence<int, 1, 9, -4, 2, -2>>();

    execute<fill_t<3, int>, std::tuple<int, int, int>>();
    execute<fill_t<5, int_<2>>, std::integer_sequence<int, 2, 2, 2, 2, 2>>();
    execute<fill_c<5, 2>, std::integer_sequence<int, 2, 2, 2, 2, 2>>();

    execute<cycle_t<3, int, double>, std::tuple<int, double, int, double, int, double>>();
    execute<cycle_t<3, int_<2>, int_<4>>, std::integer_sequence<int, 2, 4, 2, 4, 2, 4>>();
    execute<cycle_c<3, 2, 4>, std::integer_sequence<int, 2, 4, 2, 4, 2, 4>>();

    execute<rotate_t<0, 2, 5, std::tuple<int, char, double, float, int64_t>>,
            std::tuple<double, float, int64_t, int, char>>();
    execute<rotate_t<0, 2, 5, std::integer_sequence<int, 1, 2, 3, 4, 5>>,
            std::integer_sequence<int, 3, 4, 5, 1, 2>>();
    execute<rotate_t<2, 4, 7, std::integer_sequence<int, 9, 8, 1, 2, 3, 4, 5, 7, 6>>,
            std::integer_sequence<int, 9, 8, 3, 4, 5, 1, 2, 7, 6>>();

    execute<shift_left_t<2, std::tuple<int, char, double, float, int64_t>>,
            std::tuple<double, float, int64_t, int, char>>();
    execute<shift_left_t<2, std::integer_sequence<int, 1, 2, 3, 4, 5>>,
            std::integer_sequence<int, 3, 4, 5, 1, 2>>();

    execute<shift_right_t<2, std::tuple<int, char, double, float, int64_t>>,
            std::tuple<float, int64_t, int, char, double>>();
    execute<shift_right_t<2, std::integer_sequence<int, 1, 2, 3, 4, 5>>,
            std::integer_sequence<int, 4, 5, 1, 2, 3>>();

    execute<zero_sequence_t<int, 3>, std::integer_sequence<int, 0, 0, 0>>();

    execute<integer_sequence_t<int, 3, 2, 0>, std::integer_sequence<int, 2, 2, 2>>();
    execute<integer_sequence_t<int, 4, 3, 1>, std::integer_sequence<int, 3, 4, 5, 6>>();
    execute<integer_sequence_t<int, 6, 5, -1>, std::integer_sequence<int, 5, 4, 3, 2, 1, 0>>();

    execute<odd_t<std::tuple<int, char, double, int, char>>, std::tuple<char, int>>();
    execute<odd_t<std::integer_sequence<int, 1, 9, 0, 4, 2>>, std::integer_sequence<int, 9, 4>>();

    execute<even_t<std::tuple<int, char, double, int, char>>, std::tuple<int, double, char>>();
    execute<even_t<std::integer_sequence<int, 1, 9, 0, 4, 2>>, std::integer_sequence<int, 1, 0, 2>>();

    execute<count_v<int_<1>, std::integer_sequence<int, 1, 2, 3, 1>>, 2>();
    execute<count_v<char, std::tuple<char, double, char, int, float, char, int, double>>, 3>();

    execute<count_if_v<is_even, std::integer_sequence<int, 1, 2, 3, 1, 4>>, 2>();
    execute<count_if_v<is_tuple, std::tuple<double, std::tuple<>, float, std::tuple<int>, double>>, 2>();

    execute<mode_v<std::integer_sequence<int, 1, 2, 3, 1>>, 1>();
    execute<monster::mode_t<std::integer_sequence<int, 1, 2, 3, 1>>, int_<1>>();
    execute<monster::mode_t<std::tuple<char, double, char, double, int, float, char>>, char>();

    execute<transform_t<pointer_type_of, std::tuple<int, double, char*>>,
            std::tuple<int*, double*, char**>>();

    execute<transform_if_t<succ, is_even, std::integer_sequence<int>,
            std::integer_sequence<int, 0, 3, 1, 2, 8, 4>>, std::integer_sequence<int, 1, 3, 9, 5>>();

    execute<transform_while_t<succ, is_even, std::integer_sequence<int>,
            std::integer_sequence<int, 0, 2, 4, 2, 1, 8, 4>>, std::integer_sequence<int, 1, 3, 5, 3>>();

    execute<is_palindromic_v<std::tuple<int, double, int, int, double, int>>>();
    execute<is_palindromic_v<std::integer_sequence<int, 3, 0, 2, 1, 2, 0, 3>>>();
    execute<!is_palindromic_v<std::tuple<int, double, char, int, int, double, int>>>();
    execute<!is_palindromic_v<std::integer_sequence<int, 0, 2, 1, 2, 0, 3>>>();

    execute<subset_v<std::integer_sequence<int, 4>, std::integer_sequence<int, 4>>>();
    execute<!subset_v<std::integer_sequence<int, 4, 5, 6>, std::integer_sequence<int, 4>>>();
    execute<subset_v<std::integer_sequence<int, 4, 5, 6>,
            std::integer_sequence<int, 1, 2, 3, 4, 5, 6, 7>>>();
    execute<subset_v<std::tuple<float, char, double>,
            std::tuple<int, double, char, float, char, double, int>>>();

    execute<cartesian_product_t<std::tuple<int, double>, std::tuple<char, float>>,
            std::tuple<int, char, int, float, double ,char, double, float>>();
    execute<cartesian_product_t<std::integer_sequence<int, 1, 2>, std::integer_sequence<int, 3, 4, 5>>,
            std::integer_sequence<int, 1, 3, 1, 4, 1, 5, 2, 3, 2, 4, 2, 5>>();

    execute<tokenize_t<19073>, std::integer_sequence<int, 1, 9, 0, 7, 3>>();
    execute<tokenize_t<19073, 100>, std::integer_sequence<int, 1, 90, 73>>();
    execute<tokenize_t<19073, 1000>, std::integer_sequence<int, 19, 73>>();

    execute<large_number_multiplier_t<tokenize_t<975>, tokenize_t<6084>>,
            std::integer_sequence<int, 5, 9, 3, 1, 9, 0, 0>>();

    execute<prefix_t<std::tuple<int, double>, float, char>,
            std::tuple<float, char, int, float, char, double>>();
    execute<prefix_t<std::integer_sequence<int, 1, 2>, int_<3>, int_<4>>,
            std::integer_sequence<int, 3, 4, 1, 3, 4, 2>>();
    execute<prefix_c<std::integer_sequence<int, 1, 2>, 3, 4>,
            std::integer_sequence<int, 3, 4, 1, 3, 4, 2>>();

    execute<suffix_t<std::tuple<int, double>, float, char>,
            std::tuple<int, float, char, double, float, char>>();
    execute<suffix_t<std::integer_sequence<int, 1, 2>, int_<3>, int_<4>>,
            std::integer_sequence<int, 1, 3, 4, 2, 3, 4>>();
    execute<suffix_c<std::integer_sequence<int, 1, 2>, 3, 4>,
            std::integer_sequence<int, 1, 3, 4, 2, 3, 4>>();

    execute<zip_t<std::tuple<int, double>, std::tuple<char, float, nullptr_t>>,
            std::tuple<int, char, double, float>>();
    execute<zip_t<std::integer_sequence<int, 1>, std::integer_sequence<int, 3>>,
            std::integer_sequence<int, 1, 3>>();
    execute<zip_t<std::integer_sequence<int, 1, 2>, std::integer_sequence<int, 3, 4, 5>>,
            std::integer_sequence<int, 1, 3, 2, 4>>();

    execute<zip_with_t<multiplies_t, std::integer_sequence<int, 1, 2, -3>,
            std::integer_sequence<int, 3, 4, 5>>, std::integer_sequence<int, 3, 8, -15>>();
    execute<zip_with_t<pair_t, std::tuple<int, char, short, double>, std::tuple<char, double, float>>,
            std::tuple<pair_t<int, char>, pair_t<char, double>, pair_t<short, float>>>();

    execute<next_permutation_t<std::tuple<char, int, double>>, std::tuple<char, double, int>>();
    execute<next_permutation_t<std::tuple<char, double, int>>, std::tuple<int, char, double>>();
    execute<next_permutation_t<std::tuple<int, char, double>>, std::tuple<int, double, char>>();
    execute<next_permutation_t<std::tuple<int, double, char>>, std::tuple<double, char, int>>();
    execute<next_permutation_t<std::tuple<double, char, int>>, std::tuple<double, int, char>>();
    execute<next_permutation_t<std::tuple<double, int, char>>, std::tuple<double, int, char>>();

    using list_t = std::tuple<std::tuple<char, int, double>,
                              std::tuple<char, double, int>,
                              std::tuple<int, char, double>,
                              std::tuple<int, double, char>,
                              std::tuple<double, char, int>,
                              std::tuple<double, int, char>>;

    execute<matrix_element_t<3, 1, list_t>, double>();

    execute<transpose_t<list_t>, std::tuple<std::tuple<char, char, int, int, double, double>,
            std::tuple<int, double, char, double, char, int>,
            std::tuple<double, int, double, char, int, char>>>();

    execute<next_permutation_list<std::tuple<char, int, double>>, list_t>();
    execute<prev_permutation_list<std::tuple<double, int, char>>, reverse_t<list_t>>();

    execute<loop_permutation_t<std::tuple<char, int, double>>, list_t>();
    execute<loop_permutation_t<std::tuple<double, int, char>>, reverse_t<list_t>>();

    using list_n = std::tuple<std::integer_sequence<int, 1, 2, 3>,
                              std::integer_sequence<int, 1, 3, 2>,
                              std::integer_sequence<int, 2, 1, 3>,
                              std::integer_sequence<int, 2, 3, 1>,
                              std::integer_sequence<int, 3, 1, 2>,
                              std::integer_sequence<int, 3, 2, 1>>;

    execute<next_permutation_list<std::integer_sequence<int, 1, 2, 3>>, list_n>();
    execute<prev_permutation_list<std::integer_sequence<int, 3, 2, 1>>, reverse_t<list_n>>();

    execute<loop_permutation_t<std::integer_sequence<int, 1, 2, 3>>, list_n>();
    execute<loop_permutation_t<std::integer_sequence<int, 3, 2, 1>>, reverse_t<list_n>>();

    execute<next_partial_permutation_list<3, std::tuple<char, int, double>>, list_t>();
    execute<prev_partial_permutation_list<3, std::tuple<double, int, char>>, reverse_t<list_t>>();

    using list_s = std::tuple<std::tuple<char, int>,
                              std::tuple<char, double>,
                              std::tuple<int, char>,
                              std::tuple<int, double>,
                              std::tuple<double, char>,
                              std::tuple<double, int>>;

    execute<next_partial_permutation_list<2, std::tuple<char, int, double>>, list_s>();
    execute<prev_partial_permutation_list<2, std::tuple<double, int, char>>, reverse_t<list_s>>();

    execute<loop_permutation_t<std::tuple<char, int, double>, 2>, list_s>();
    execute<loop_permutation_t<std::tuple<double, int, char>, 2>, reverse_t<list_s>>();

    execute<permutation_recursive_t<std::integer_sequence<int, 1, 2, 3>>,
            std::tuple<std::integer_sequence<int, 1, 2, 3>,
                       std::integer_sequence<int, 1, 3, 2>,
                       std::integer_sequence<int, 2, 1, 3>,
                       std::integer_sequence<int, 2, 3, 1>,
                       std::integer_sequence<int, 3, 2, 1>,
                       std::integer_sequence<int, 3, 1, 2>>>();

    execute<permutation_recursive_t<std::tuple<char, int, double>>,
            std::tuple<std::tuple<char, int, double>,
                       std::tuple<char, double, int>,
                       std::tuple<int, char, double>,
                       std::tuple<int, double, char>,
                       std::tuple<double, int, char>,
                       std::tuple<double, char, int>>>();

    using comb0 = std::tuple<char, short, int, double>;
    execute<range_t<0, 2, comb0>, std::tuple<char, short>>();
    using comb1 = next_combination_t<2, comb0>;
    execute<range_t<0, 2, comb1>, std::tuple<char, int>>();
    using comb2 = next_combination_t<2, comb1>;
    execute<range_t<0, 2, comb2>, std::tuple<char, double>>();
    using comb3 = next_combination_t<2, comb2>;
    execute<range_t<0, 2, comb3>, std::tuple<short, int>>();
    using comb4 = next_combination_t<2, comb3>;
    execute<range_t<0, 2, comb4>, std::tuple<short, double>>();
    using comb5 = next_combination_t<2, comb4>;
    execute<range_t<0, 2, comb5>, std::tuple<int, double>>();

    using comb_list = std::tuple<std::tuple<char, short>,
                                 std::tuple<char, int>,
                                 std::tuple<char, double>,
                                 std::tuple<short, int>,
                                 std::tuple<short, double>,
                                 std::tuple<int, double>>;

    execute<next_combination_list<2, std::tuple<char, short, int, double>>, comb_list>();

    execute<prev_combination_list<2, reverse_t<comb0>>,
            std::tuple<std::tuple<double, int>,
                       std::tuple<double, short>,
                       std::tuple<double, char>,
                       std::tuple<int, short>,
                       std::tuple<int, char>,
                       std::tuple<short, char>>>();

    execute<next_combination_list<2, std::integer_sequence<int, 1, 2, 3, 4>>,
            std::tuple<std::integer_sequence<int, 1, 2>,
                       std::integer_sequence<int, 1, 3>,
                       std::integer_sequence<int, 1, 4>,
                       std::integer_sequence<int, 2, 3>,
                       std::integer_sequence<int, 2, 4>,
                       std::integer_sequence<int, 3, 4>>>();

    execute<next_combination_list<3, std::integer_sequence<int, 1, 2, 3, 4, 5>>,
            std::tuple<std::integer_sequence<int, 1, 2, 3>,
                       std::integer_sequence<int, 1, 2, 4>,
                       std::integer_sequence<int, 1, 2, 5>,
                       std::integer_sequence<int, 1, 3, 4>,
                       std::integer_sequence<int, 1, 3, 5>,
                       std::integer_sequence<int, 1, 4, 5>,
                       std::integer_sequence<int, 2, 3, 4>,
                       std::integer_sequence<int, 2, 3, 5>,
                       std::integer_sequence<int, 2, 4, 5>,
                       std::integer_sequence<int, 3, 4, 5>>>();

    using counts_list = std::tuple<std::integer_sequence<int, 0, 0, 2>,
                                   std::integer_sequence<int, 0, 1, 1>,
                                   std::integer_sequence<int, 0, 2, 0>,
                                   std::integer_sequence<int, 1, 0, 1>,
                                   std::integer_sequence<int, 1, 1, 0>,
                                   std::integer_sequence<int, 2, 0, 0>>;

    execute<next_combination_counts_list<0, 3, std::integer_sequence<int, 0, 0, 2>>, counts_list>();
    execute<prev_combination_counts_list<0, 3, std::integer_sequence<int, 2, 0, 0>>,
            reverse_t<counts_list>>();

    using integer = integer_sequence_t<int, 3, 1, 0>;

    using indices_list = std::tuple<std::integer_sequence<int, 1, 1, 1>,
                                    std::integer_sequence<int, 1, 1, 2>,
                                    std::integer_sequence<int, 1, 2, 1>,
                                    std::integer_sequence<int, 1, 2, 2>,
                                    std::integer_sequence<int, 2, 1, 1>,
                                    std::integer_sequence<int, 2, 1, 2>,
                                    std::integer_sequence<int, 2, 2, 1>,
                                    std::integer_sequence<int, 2, 2, 2>>;

    execute<next_hypercube_indices_list<1, 3, integer>, indices_list>();
    execute<prev_hypercube_indices_list<1, 3, integer_sequence_t<int, 3, 2, 0>>, reverse_t<indices_list>>();

    execute<power_v<10, digit_v<1234>>, 10000>();

    /*
    execute<edit_distance_v<std::tuple<char, short, float, int, double, uint64_t>,
            std::tuple<char, double, nullptr_t, short, uint32_t, int, double, uint64_t>>, 3>();

    execute<edit_distance_v<std::integer_sequence<int, 1, 2, 3, 4, 5, 6>,
            std::integer_sequence<int, 1, 5, 8, 2, 0, 4, 5, 6>>, 3>();
    */

    execute<binomial_coeff_v<5, 2>, 10>();

    execute<select_t<2, std::tuple<short, int, double, int, char>>, short>();
    execute<select_t<3, std::integer_sequence<int, 0, 3, 2, 1, 2, 6>>, int_<2>>();
    execute<select_v<4, std::integer_sequence<int, -2, 1, 0, -7, 4, 3>>, 1>();

    execute<select_t<2, std::tuple<short, int, double, int, char>, greater_equal_t>, int>();
    execute<select_t<3, std::integer_sequence<int, 0, 3, 4, 1, -2, 6>, greater_equal_t>, int_<3>>();
    execute<select_v<4, std::integer_sequence<int, -2, 1, 0, -7, 4, 3>, greater_equal_t>, 0>();

    execute<stable_partition_t<0, 2, std::tuple<int, std::tuple<int>>, is_tuple>,
            std::tuple<std::tuple<int>, int>>();

    execute<stable_partition_t<1, 6, std::tuple<int, char, std::tuple<char>, double,
            std::tuple<uint64_t>, int, std::tuple<float>>, is_tuple>, std::tuple<int, std::tuple<char>,
            std::tuple<uint64_t>, char, double, int, std::tuple<float>>>();

    execute<stable_partition_t<1, 7, std::integer_sequence<int, 1, 3, 2, 7, 8, 0, 1, -2, 4, 5>, is_odd>,
            std::integer_sequence<int, 1, 3, 7, 1, 2, 8, 0, -2, 4, 5>>();

    execute<stable_partition_t<1, 6, std::integer_sequence<int, 1, 3, 2, 7, 8, 0, 1, -2, 4, 5>, is_even>,
            std::integer_sequence<int, 1, 2, 8, 0, 3, 7, 1, -2, 4, 5>>();

    execute<partition_point_t<std::integer_sequence<int, 1, 2, 8, 0, 3, 7, 1, -2, 4, 5>,
            is_even, 1, 6>, int_<4>>();

    execute<partition_point_v<std::tuple<std::tuple<int>, std::tuple<int>, char, int>, is_tuple>, 2>();

    execute<stable_sort_t<std::tuple<double, uint64_t, double, float, int, char, float, char, double>>,
            std::tuple<char, char, float, int, float, double, uint64_t, double, double>>();

    execute<select_sort_t<std::tuple<int, int, double, char, int, char, double>>,
            std::tuple<char, char, int, int, int, double, double>>();

    execute<counting_sort_t<std::tuple<int, char, double, uint64_t, int>>,
            std::tuple<char, int, int, double, uint64_t>>();

    execute<radix_sort_t<std::tuple<int, char, double, uint64_t, int>>,
            std::tuple<char, int, int, double, uint64_t>>();

    execute<stooge_sort_t<std::tuple<int, double, int, float, char, double>>,
            std::tuple<char, int, int, float, double, double>>();

    execute<bubble_sort_t<std::tuple<int, double, char, int, char, float, double>>,
            std::tuple<char, char, int, int, float, double, double>>();

    execute<shaker_sort_t<std::tuple<uint64_t, int, double, char, int, char, float, double>>,
            std::tuple<char, char, int, int, float, uint64_t, double, double>>();

    execute<oddeven_sort_t<std::tuple<uint64_t, int, double, char, int, char, float, double>>,
            std::tuple<char, char, int, int, float, uint64_t, double, double>>();

    execute<gnome_sort_t<std::tuple<uint64_t, int, double, char, int, char, float, double>>,
            std::tuple<char, char, int, int, float, uint64_t, double, double>>();

    execute<selection_sort_t<std::tuple<int, int, double, char, int, char, double>>,
            std::tuple<char, char, int, int, int, double, double>>();

    execute<quick_sort_t<std::tuple<double, uint64_t, float, char, char, double>>,
            std::tuple<char, char, float, double, uint64_t, double>>();

    execute<quick_sort_iterative_t<std::tuple<double, uint64_t, float, char, char, double>>,
            std::tuple<char, char, float, double, uint64_t, double>>();

    execute<insert_sort_t<std::tuple<float, int, int, double, char, int, float, char, double>>,
            std::tuple<char, char, float, int, int, int, float, double, double>>();

    execute<insertion_sort_t<std::tuple<float, int, int, double, char, int, float, char, double>>,
            std::tuple<char, char, float, int, int, int, float, double, double>>();

    execute<merge_sort_t<std::tuple<double, char, double, int, float>>,
            std::tuple<char, int, float, double, double>>();

    execute<strand_sort_t<std::tuple<double, char, double, int, float>>,
            std::tuple<char, int, float, double, double>>();

    execute<heap_sort_t<std::tuple<uint64_t, double, float, char, char, double>>,
            std::tuple<char, char, float, double, double, uint64_t>>();

    using input1 = std::integer_sequence<int, 2, 1, 0, -3, 4, 1, -7, 5, -2>;
    using output1 = std::integer_sequence<int, -7, -3, -2, 0, 1, 1, 2, 4, 5>;
    using index1 = std::index_sequence<6, 3, 8, 2, 1, 5, 0, 4, 7>;

    execute<stable_sort_t<input1>, output1>();
    execute<select_sort_t<input1>, output1>();
    execute<counting_sort_t<input1>, output1>();
    execute<radix_sort_t<input1>, output1>();
    execute<stooge_sort_t<input1>, output1>();
    execute<bubble_sort_t<input1>, output1>();
    execute<shaker_sort_t<input1>, output1>();
    execute<oddeven_sort_t<input1>, output1>();
    execute<gnome_sort_t<input1>, output1>();
    execute<selection_sort_t<input1>, output1>();
    execute<quick_sort_t<input1>, output1>();
    execute<quick_sort_iterative_t<input1>, output1>();
    execute<insert_sort_t<input1>, output1>();
    execute<insertion_sort_t<input1>, output1>();
    execute<merge_sort_t<input1>, output1>();
    execute<strand_sort_t<input1>, output1>();
    execute<heap_sort_t<input1>, output1>();
    execute<sort_index_t<input1>, index1>();

    execute<sort_v<input1, output1, stable_sort, select_sort, counting_sort,
            radix_sort, stooge_sort, bubble_sort, oddeven_sort, gnome_sort,
            shaker_sort, selection_sort, quick_sort, quick_sort_iterative,
            insert_sort, insertion_sort, merge_sort, strand_sort, heap_sort>>();

    using input2 = std::tuple<double, short, double, int, char, char, double>;
    using output2 = std::tuple<char, char, short, int, double, double, double>;
    using index2 = std::index_sequence<4, 5, 1, 3, 6, 2, 0>;

    execute<stable_sort_t<input2>, output2>();
    execute<select_sort_t<input2>, output2>();
    execute<counting_sort_t<input2>, output2>();
    execute<radix_sort_t<input2>, output2>();
    execute<stooge_sort_t<input2>, output2>();
    execute<bubble_sort_t<input2>, output2>();
    execute<shaker_sort_t<input2>, output2>();
    execute<oddeven_sort_t<input2>, output2>();
    execute<gnome_sort_t<input2>, output2>();
    execute<selection_sort_t<input2>, output2>();
    execute<quick_sort_t<input2>, output2>();
    execute<quick_sort_iterative_t<input2>, output2>();
    execute<insert_sort_t<input2>, output2>();
    execute<insertion_sort_t<input2>, output2>();
    execute<merge_sort_t<input2>, output2>();
    execute<strand_sort_t<input2>, output2>();
    execute<heap_sort_t<input2>, output2>();
    execute<sort_index_t<input2>, index2>();

    execute<sort_v<input2, output2, stable_sort, select_sort, counting_sort,
            radix_sort, stooge_sort, bubble_sort, oddeven_sort, gnome_sort,
            shaker_sort, selection_sort, quick_sort, quick_sort_iterative,
            insert_sort, insertion_sort, merge_sort, strand_sort, heap_sort>>();

    return 0;
}
```
