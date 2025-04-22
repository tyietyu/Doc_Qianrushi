**1、循环队列（Circular Buffer）**

 

typedef struct {

   int buffer[SIZE];

   int head;

   int tail;

   int count;

} CircularBuffer;

 

void push(CircularBuffer *cb, int data) {

   if (cb->count < SIZE) {

​     cb->buffer[cb->head] = data;

​     cb->head = (cb->head + 1) % SIZE;

​     cb->count++;

   }

}

 

int pop(CircularBuffer *cb) {

   if (cb->count > 0) {

​     int data = cb->buffer[cb->tail];

​     cb->tail = (cb->tail + 1) % SIZE;

​     cb->count--;

​     return data;

   }

   return -1; // Buffer is empty

}

 

**2、断言（Assertion）**

\#define assert(expression) ((void)0)

\#ifndef NDEBUG

\#undef assert

\#define assert(expression) ((expression) ? (void)0 : assert_failed(__FILE__, __LINE__))

\#endif

 

void assert_failed(const char *file, int line) {

   printf("Assertion failed at %s:%d\n", file, line);

   // Additional error handling or logging can be added here

}

 

**3、位域反转（Bit Reversal）**

 

unsigned int reverse_bits(unsigned int num) {

   unsigned int numOfBits = sizeof(num) * 8;

   unsigned int reverseNum = 0;

 

   for (unsigned int i = 0; i < numOfBits; i++) {

​     if (num & (1 << i)) {

​       reverseNum |= (1 << ((numOfBits - 1) - i));

​     }

   }

   return reverseNum;

}

 

**4、固定点数运算（Fixed-Poin Arithmetic）**

typedef int16_t fixed_t;

 

\#define FIXED_SHIFT 8

\#define FLOAT_TO_FIXED(f) ((fixed_t)((f) * (1 << FIXED_SHIFT)))

\#define FIXED_TO_FLOAT(f) ((float)(f) / (1 << FIXED_SHIFT))

 

fixed_t fixed_multiply(fixed_t a, fixed_t b) {

   return (fixed_t)(((int32_t)a * (int32_t)b) >> FIXED_SHIFT);

}

 

**5、字节序转换（Endianness Conversion）**

uint16_t swap_bytes(uint16_t value) { return (value >> 8) | (value << 8); }

 

**6、位掩码（Bit Masks）**

\#define BIT_MASK(bit) (1 << (bit))

 

**7、计数器计数（Timer Counting）**

\#include <avr/io.h>

void setup_timer() {

   // Configure timer settings

}

uint16_t read_timer() {

   return TCNT1;

}

 

**8、二进制查找（Binary Search）**

int binary_search(int arr[], int size, int target) {

   int left = 0, right = size - 1;

 

   while (left <= right) {

​     int mid = left + (right - left) / 2;

​     if (arr[mid] == target) {

​       return mid;

​     } else if (arr[mid] < target) {

​       left = mid + 1;

​     } else {

​       right = mid - 1;

​     }

   }

   return -1; // Not found

}

 

**9、位集合（Bitset）**

\#include <stdint.h>

typedef struct {

   uint32_t bits;

} Bitset;

 

void set_bit(Bitset *bitset, int bit) {

   bitset->bits |= (1U << bit);

}

 

int get_bit(Bitset *bitset, int bit) {

   return (bitset->bits >> bit) & 1U;

}

 

来自 <https://mp.weixin.qq.com/s/tYRbkDQ6louwEKhBMqc3eQ> 