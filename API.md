# veque API
_The double-ended vector_

------


    template <typename T, typename Allocator = std::allocator<T> >
    class veque
    
## Iterator invalidation

| Operations | Invalidated |
|---|---|
| All read only operations, swap | Never |
| clear, operator=, assign | Always |
| insert, emplace | **Always**.   |
| all reserves, shrink_to_fit | If the vector changed capacity, all of them. If not, none |
| push_back, emplace_back, resize, resize_back | If the vector changed capacity, all of them. If not, only end() |
| push_front, emplace_front, resize_front | If the vector changed capacity, all of them. If not, only begin() |
| All resizes | If the vector changed capacity, all of them. If not, only begin() or end() |
| All pop_backs | The element erased and end() |
| All pop_fronts | The element erased and begin() |

## Member types
  
        using allocator_type         = Allocator;
        using value_type             = T;
        using reference              = T &;
        using const_reference        = const T &;
        using pointer                = T *;
        using const_pointer          = const T *;
        using iterator               =  T *;
        using const_iterator         = const T *;
        using reverse_iterator       = std::reverse_iterator<iterar>;
        using const_reverse_iterator = std::reverse_iterator<const_iterator>;
        using difference_type        = std::ptrdiff_t;
        using size_type              = std::size_t;
        using ssize_type             = std::ptrdiff_t;

## Member functions

All constructors match the behavior, complexity, and exception rules of [C++17 `std::vector` constructors](https://en.cppreference.com/w/cpp/container/vector/vector)

        veque() noexcept (noexcept(Allocator()));
        explicit veque( const Allocator& ) noexcept;
        explicit veque( size_type n, const Allocator& = Allocator() );
        veque( size_type n, const T &val, const Allocator& = Allocator() );
        template <typename InputIt>
        veque( InputIt first,  InputIt last, const Allocator& = Allocator() );
        veque( std::initializer_list<T>, const Allocator& = Allocator() );
        veque( const veque & );
        veque( const veque &, const Allocator& );
        veque( veque && ) noexcept;
        veque( veque &&, const Allocator& ) noexcept;
        
Destructs the container. The destructors of the elements are called and the used storage is deallocated.

        ~veque();
        
All assignment operators match the behavior, complexity, and exception rules of [C++17 `std::vector` assignment operators](https://en.cppreference.com/w/cpp/container/vector/operator%3D)

        veque & operator=(const veque &);
        veque & operator=(veque &&) noexcept(
            noexcept(std::allocator_traits<Allocator>::propagate_on_container_move_assignment::value
            || std::allocator_traits<Allocator>::is_always_equal::value) );
        veque & operator=(std::initializer_list<T>);
        
All `assign` functions match the behavior, complexity, and exception rules of [C++17 `std::vector::assign`](https://en.cppreference.com/w/cpp/container/vector/assign)

        void assign(size_type, const T &value);
        void assign(iterator, iterator);
        void assign(std::initializer_list<T>);
        allocator_type get_allocator() const;

### Element access
        reference at(size_type);
        const_reference at(size_type) const;
        reference operator[](size_type) ;
        const_reference operator[](size_type) const;
        reference front();
        const_reference front() const;
        reference back();
        const_reference back() const;
        T * data() noexcept;
        const T * data() const noexcept;
        
### Iterators
        iterator begin() noexcept;
        const_iterator begin() const noexcept;
        const_iterator cbegin() const noexcept;
        iterator end() noexcept;
        const_iterator end() const noexcept;
        const_iterator cend() const noexcept;
        reverse_iterator rbegin() noexcept;
        const_reverse_iterator rbegin() const noexcept;
        const_reverse_iterator crbegin() const noexcept;
        reverse_iterator rend() noexcept;
        const_reverse_iterator rend() const noexcept;
        const_reverse_iterator crend() const noexcept;

### Capacity
        [[nodiscard]] bool empty() const noexcept;
        size_type size() const noexcept;
        ssize_type ssize() const noexcept;
        size_type max_size() const noexcept;
        void reserve(size_type);
        void reserve_front(size_type);
        void reserve_back(size_type);
        // Returns current storage + unused allocated storage before front()
        size_type capacity_front() const noexcept;
        // Returns current storage + unused allocated storage after back()
        size_type capacity_back() const noexcept;
        // Returns current storage + all unused allocated storage
        size_type capacity_full() const noexcept;
        // To achieve interface parity with std::vector, capacity() returns capacity_back();
        size_type capacity() const noexcept;
        void shrink_to_fit();

### Modifiers
        void clear() noexcept;
        iterator insert(const_iterator, const T &);
        iterator insert(const_iterator, T &&);
        iterator insert(const_iterator, size_type, const T&);
        template <class InputIt> iterator insert(const_iterator, InputIt, InputIt);
        iterator insert(const_iterator, std::initializer_list<T>);
        template <class ... Args> iterator emplace(const_iterator, Args && ...);
        iterator erase(const_iterator);
        iterator erase(const_iterator, const_iterator);
        void push_back(const T &);
        void push_back(T &&);
        template <class ... Args> reference emplace_back(Args && ... args);
        void push_front(const T &);
        void push_front(T &&);
        template <class ... Args> reference emplace_front(Args && ... args);
        void pop_back();
        // Move-savvy pop back with strong exception guarantee
        T pop_back_instance();
        void pop_front();
        // Move-savvy pop front with strong exception guarantee
        T pop_front_instance();
        // Resizes the veque, by adding or removing from the front. 
        void resize_front(size_type);
        void resize_front(size_type, const T &);
        // Resizes the veque, by adding or removing from the back.
        void resize_back(size_type);
        void resize_back(size_type, const T &);
        // To achieve interface parity with std::vector, resize() performs resize_back();
        void resize(size_type);
        void resize(size_type, const T &);
        void swap(veque &) noexcept(
            noexcept(std::allocator_traits<Allocator>::propagate_on_container_swap::value
            || std::allocator_traits<Allocator>::is_always_equal::value));

## Non-member functions

    bool operator==(const veque<T,Alloc> &lhs, const veque<T,Alloc> &rhs)
    bool operator!=(const veque<T,Alloc> &lhs, const veque<T,Alloc> &rhs)
    bool operator<(const veque<T,Alloc> &lhs, const veque<T,Alloc> &rhs)
    bool operator<=(const veque<T,Alloc> &lhs, const veque<T,Alloc> &rhs)
    bool operator>(const veque<T,Alloc> &lhs, const veque<T,Alloc> &rhs)
    bool operator>=(const veque<T,Alloc> &lhs, const veque<T,Alloc> &rhs)

    void swap(veque<T,Alloc> & lhs, veque<T,Alloc> & rhs) noexcept(
        noexcept(std::allocator_traits<Alloc>::propagate_on_container_swap::value
        || std::allocator_traits<Alloc>::is_always_equal::value))

## Non-member types

    struct std::hash<veque<T,Alloc>>

## Deduction Guides

    template< class InputIt, class Alloc = std::allocator<typename std::iterator_traits<InputIt>::value_type>>
    veque(InputIt, InputIt, Alloc = Alloc())
      -> veque<typename std::iterator_traits<InputIt>::value_type, Alloc>;