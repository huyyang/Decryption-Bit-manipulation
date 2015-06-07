#include <stdlib.h>
#include <stdio.h>
#include <stdint.h>
//prototypes
void printBytes(unsigned char * getDecrypted);				//prints bytes
unsigned char * decryptStageOne(unsigned char * getDecrypted);		//stage one: Swap bytes c0 and c1
unsigned char * decryptStageTwo(unsigned char * getDecrypted);		//stage two: Swap bits b7 and b1, swap bits b6 and b0
unsigned char * decryptStageThree(unsigned char * getDecrypted);	//stage three: XOR bytes c1 and c3 with k3
unsigned char * readIn();						//read in from file
void translation(unsigned char * getFullDecrypted);			//displays our end result

//our main function
int main(int argc, char * argv[]){

	unsigned char * getDecrypted = readIn();				//catches whatever we read in from our read in function 
	printBytes(getDecrypted);						//prints out the current hexadecimal representation from the read in
	unsigned char * getDecrypted2 = decryptStageOne(getDecrypted);		//executes stage 1 and stores that into the char array
	unsigned char * getDecrypted3 = decryptStageTwo(getDecrypted2);		//executes stage 2 and stores that into the char array
	unsigned char * getDecrypted4 = decryptStageThree(getDecrypted3);	//executes stage 3 and stores that into the char array
	translation(getDecrypted4);
	return 0;
}

//this is a read in function in which will read in from our file called yan2.
unsigned char * readIn(){

	//makes a file pointer and opens the file yan2
	FILE * file = fopen("yan2", "r");

	//makes a new char pointer
	unsigned char * example;

	//memory allocates the char pointer to store 100 characters
	example = malloc(100); 

	//set i to 0
	int i = 0;

	//make an unsigned integer because none of our hexadecimals are
	//negative or should be negative
	unsigned int x;

	//while x gets the value of whatever is in the file
	while((x = fgetc(file)) != EOF){

		//we increment up our allocated character to each index and 
		//set it to the unsigned casted character
		example[i++] = (unsigned char) x;
	}

	//after we are all done we are returning our character so later we
	//can catch the string in another function
	return example;
}

//this function will display the bytes in hexadecimal
void printBytes(unsigned char * getDecrypted){

	printf("Here is your current byte representation\n");

	//integer for our for loop
	int i;

	//we wil loop thru the array 80 times
	for(i = 0; i < 80; ++i){	

		//prints the unsigned char of getDecrypted in hexadecimal
		//we represent x as the hexadevimal representation
		printf("%x ", (unsigned char) getDecrypted[i]);		

	}

	printf("\n");
}

//Everytime there is 4 bytes, what this function will do since my key is "Y"
//is it will switch the first byte with the second byte
unsigned char * decryptStageOne(unsigned char * getDecrypted){

	//getEncrypted gets the passed in character of getDecrypted
	unsigned char * getEncrypted = getDecrypted;

	//an integer made for the for loop
	int i;

	//a for loop that will loop 80 times thru the 80 bytes
	for(i = 0; i < 80; ++i){

		//everytime we have a number that is divisible by 4 (aka. everytime we 
		//hit 4 bytes)
		if(i % 4 == 0){

			//set temp to the index so we can swap
			unsigned char temp = getEncrypted[i];

			//the first index will now get the index to the next
			getEncrypted[i] = getEncrypted[i + 1];

			//then the second index will get temp in which is the
			//first index byte
			getEncrypted[i + 1] = temp;
		}
	}
	//prints out the current byte representation
	printBytes(getEncrypted);

	//returns the byte representation
	return getEncrypted;
}

//this fucntion will take in an unsigned character and will return an unsigned
//character in the end
unsigned char * decryptStageTwo(unsigned char * getDecrypted){

	//character gets the unsigned character that is being passed in
	unsigned char * getEncrypted = getDecrypted;

	//an integer variable that is going to be used for the for loop
	int i;

	//for loop that will loop through 
	for(i = 0; i < 80; ++i){

		//declare an unsigned integer variable that is equivalent
		//to whatever is the index of the character	
		uint8_t newEncrypted = getEncrypted[i];

		//this process is called bit masking. We are going to be
		//turning on and off bits (negation) to get the results that
		//we want, depending on what the binary value is 
		if(getEncrypted[i] & 0x80)	//if 0x02 is turned on then 0x80 is on
			newEncrypted |= 0x02;	//the result that is turned on
		else				//if 0x02 is turned off, then 0x80 is off
			newEncrypted &= 0xFD;	//the result that is turned off
		
		if(getEncrypted[i] & 0x02)	//if 0x80 is turned on, then 0x02 is on
			newEncrypted |= 0x80;	//the result that is turned on
		else				//if 0x80 is turned off, then 0x02 is off
			newEncrypted &= 0x7F;	//the result that is turned off
		
		if(getEncrypted[i] & 0x40)	//if 0x40 is turned on then 0x01 is on
			newEncrypted |= 0x01;	//the result that is turned on
		else				//if 0x40 is turned off then 0x01 is off
			newEncrypted &= 0xFE;	//the result that is turned off
		
		if(getEncrypted[i] & 0x01)	//if 0x01 is turned on then 0x40 is on
			newEncrypted |= 0x40;	//the result that is turned on
		else				//if ox01 is turned off then 0x40 is off
			newEncrypted &= 0xBF;	//the result that is turned off

		//getEncrypted if index i is now a newly kmodified binary value
		getEncrypted[i] = newEncrypted;	
	}
	//prints out our character
	printBytes(getEncrypted);

	//returns our character array
	return getEncrypted;	
}

//this function will take whatever is being passed into this function in which
//is the message inside of the string array and will turn the every 4 bytes of
//c1 and c3 into the xor reprentation of whatever my third letter of my key is 
//i
//which it is "n". So the hex representation of n is 0x63. Then whatever each
//of the encrypted index is, then xor 
unsigned char * decryptStageThree(unsigned char * getDecrypted){

	//store our character pointer that is being passed in, into a new 
	//character pointer
	unsigned char * getEncrypted = getDecrypted;

	//create an integer made for the for loop
	int i;

	//for loop that will loop thru the array 80 times
	for(i = 0; i < 80; ++i){

		//if the i divided 4 is 0 (Everytime we hit 4 bytes)
		if(i % 4 == 0){
			
			//the index of the array is going to be xor with 0x6E
			getEncrypted[i + 1] = getEncrypted[i + 1] ^ 0x6E; 

			//the index of the array is going to be xor with 0x6E
			getEncrypted[i + 3] = getEncrypted[i + 3] ^ 0x6E;
		}
	}
	//prints out our current bytes
	printBytes(getEncrypted);

	//returns our character array
	return getEncrypted;
}

//This function is supposed to take in an unsigned char and display the 
//character array that is being passed in
void translation(unsigned char * getFullDecrypted){

	unsigned char * sentence = getFullDecrypted;
	printf("Here is your decrypted sentence:\n");
	printf("%s\n", sentence);
}

