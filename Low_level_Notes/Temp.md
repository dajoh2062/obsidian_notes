
### 2025:


16:

```c
int y = 8;
if(y>7){
	z=3;
}
else {
	z=4*y;
}
```


17:

```c
uint8_t encode(bool sign, uint8_t exp, uint8_t mant){
	uint8t_t result = 0b00000000;
	
	uint8_t mant_mask = 0b00000111;
	uint8_t exp_mask  = 0b01111000;
	uint8_t sign_mask = 0b10000000;
	
	result |= (mant_mask & (mant));
	result |= (exp_mask & (exp <<3));
	result |= ((sign<<7) & sign_mask);
	
	return result;
}


```


18:

```c
float decode(uint8_t minifloat){

	uint8_t mant = (minifloat & 0b00000111);
	uint8_t exp  = (minifloat & 0b11111000) >> 3;
	bool sign    = (bool)((minifloat & 0b10000000)>>7);
	
	float result = absValue(exp, mant);
	
	if(sign) return -result;
	
	return result; 

}

```


19:

```c


bool addToSamples(Samples* ptr, float value){

	if (ptr->avail <= 0){
		printf("buffer is full");
		return false;
	}
	int dest = NUM_SAMPLES-(ptr->avail);
	
	ptr->values[dest] = value;
	ptr->avail--;
	return true;

}


```


20:

```c
void calculateCheckSum (Samples* ptr){
	long sum = 0;
	
	for(int i = 0; i<NUM_SAMPLES; i++){
		sum+=(long)(round((ptr->values[i])*10));
	}
	
	ptr->simpleCheckSum = sum;
}

```


21:
```c
char sendBuffer [(NUM_SAMPLES*sizeof(float)) + sizeof(long)];
void sendData(Samples* ptr){

	memcpy(sendBuffer, (ptr->values), sizeof(float)*NUM_SAMPLES);
	memcpy(sendbuffer + sizeof(float)*NUM_SAMPLES , 
	&(ptr-> simpleCheckSum),sizeof(long));
	
	ptr->avail = NUM_SAMPLES;
}
```


22:

```c

static char sendbuffer [sizeof(float) + 23 + sizeof(long)];
void sendDataCompact(Samples* ptr){

	int8_t temp [NUM_SAMPLES-1];
	
	for(int i = 1; i< NUM_SAMPLES; i++){
		temp[i-1] = (int8_t)(round(10*(ptr->values[i]-ptr->values[i-1])));
	}

	 memcpy(sendBuffer, &ptr->values[0], sizeof(float));

    memcpy(sendBuffer + sizeof(float),
           temp, sizeof(temp));

    memcpy(sendBuffer + sizeof(float) + sizeof(temp),
           &ptr->simpleCheckSum, sizeof(long));

    ptr->avail = NUM_SAMPLES;
}
```


### 2024:


20:
```c

int countNodes(Node* n) {
    int count = 0;

    while (n != NULL) {
        count++;
        n = n->next;
    }

    return count;
}
```

21:
```c
uint32_t SDLcolor(SDL_Surface *sSurf,)


```