# Project-2-_Server
Attached Project#2
*/Server Project # 2
recv after creating a socket and setting it up for broadcast
reciever too setss the destination address of -1 and 
keeps waiting for broadcast form the server in a infinite loop with a sleep/*
// server.c:
#include <sys/types.h>
#include <sys/socket.h>
#include <string.h>
#include <stdio.h>
#include <stdlib.h>
#include <netdb.h>
#include <netinet/in.h>
 
#define PORT 2089
#define DEST_ADDR "255.255.255.255" //broadcast addr
#define BUFSIZE 1024
int main(int argc, char *argv[])
{
    	int sockfd;
    	char buf[BUFSIZE];
    	struct sockaddr_in sendaddr;
    	struct sockaddr_in recvaddr;
    	int numbytes;
    	int addr_len;
    	int broadcast=1;
 
    	if((sockfd = socket(PF_INET, SOCK_DGRAM, 0)) == -1)
    	{
            	perror("socket");
            	exit(1);
    	}
   	if((setsockopt(sockfd,SOL_SOCKET,SO_BROADCAST,
                            	&broadcast,sizeof broadcast)) == -1)
    	{
            	perror("setsockopt - SO_SOCKET ");
            	exit(1);
    	}
    	if((setsockopt(sockfd,SOL_SOCKET,SO_REUSEADDR,
                            	&broadcast,sizeof broadcast)) == -1)
    	{
            	perror("setsockopt - SO_SOCKET ");
            	exit(1);
    	}
 
    	printf("Socket created\n");
 
    	sendaddr.sin_family = AF_INET;
    	sendaddr.sin_port = htons(PORT);
    	sendaddr.sin_addr.s_addr = INADDR_ANY;
    	memset(sendaddr.sin_zero,'\0', sizeof sendaddr.sin_zero);
 
 
    	recvaddr.sin_family = AF_INET;
    	recvaddr.sin_port = htons(PORT);
    	recvaddr.sin_addr.s_addr = INADDR_ANY;
    	memset(recvaddr.sin_zero,'\0',sizeof recvaddr.sin_zero);
    	if(bind(sockfd, (struct sockaddr*) &recvaddr, sizeof recvaddr) == -1)
    	{
            	perror("bind");
            	exit(1);
    	}
 
 
    	addr_len = sizeof sendaddr;
    	if ((numbytes = recvfrom(sockfd, buf, sizeof buf, 0,
          	(struct sockaddr *)&sendaddr, (socklen_t *)&addr_len)) == -1)
    	{
    	perror("recvfrom");
  	exit(1);
   }
 
    	printf("%s",buf);
 
    	return 0;
}

