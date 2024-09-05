
To enable eager loading (disable lazy loading)

![[Pasted image 20240903195252.png]]

With lazy loading you could end up call more queries than you should.
For more details watch this video again it makes use of debugbar.

https://www.youtube.com/watch?v=gaW9KODumUg&list=PL3VM-unCzF8hy47mt9-chowaHNjfkuEVz&index=13

![[Pasted image 20240903200248.png]]

If you switch off lazy loading but leave the code doing this.

![[Pasted image 20240903200351.png]]

You should see an error like this.
![[Pasted image 20240903200445.png]]

To use eager loading change query to this.

![[Pasted image 20240903200701.png]]
