helloworld: helloworld.o main.o
	gcc -o helloworld helloworld.o main.o
helloworld.o: helloworld.c
	gcc -c helloworld.c
main.o: main.c
	gcc -c main.c

clean:
	rm -f helloworld *.o
