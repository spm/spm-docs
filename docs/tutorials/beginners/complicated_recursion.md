## A More Complicated Example of Recursion
Anyone really keen to learn about recursion (I suspect very few) might wish to take a look at this code.
There are features in the code that might not be familiar yet (such as .m files consisting of multiple functions),
but reading (and understanding) examples of code can be a good way to learn. 

Copy/paste the following into a file called ``noughtcross.m``, and type ``noughtcross`` in MATLAB to run it:

```matlab
function noughtcross
% Noughts and crosses
% There are no input or output arguments

    % Set up an empty board
    G = ['   '; '   '; '   '];

    % Lets go...
    game_over(G);
    while(any(any(G==' ')))

        % User's go
        G = human(G,'X');
        if game_over(G), break; end

        % Computer's go
        G = computer(G,'O');
        if game_over(G), break; end
    end
end


function t = game_over(G)
    % Show the board and decide whether the game is finished

    % Display the board
    num = char(reshape(1:9,3,3)'+'₀');
    Gt  = G;
    Gt(G==' ') = num(G==' ');
    s   = ('   ')';
    disp(' ')
    disp([s Gt(:,1) s Gt(:,2) s Gt(:,3) s])

    % Check whether to terminate, and show the result if relevant
    f = score(G);
    t = true;
    if     f<0, disp('X wins.')
    elseif f>0, disp('O wins.')
    else
        t = isdraw(G);
        if t,   disp('Draw.'); end
    end
end


function t = isdraw(G)
    % Is it a draw?
    d1 = diag(G);
    d2 = diag(flipud(G));
    t = all(any(G=='O',1) & any(G=='X',1)) && ...
        all(any(G=='O',2) & any(G=='X',2)) && ...
        any(d1=='X') && any(d1=='O') && ...
        any(d2=='X') && any(d2=='O');
end


function G = human(G,player)
    % User enters their move and the board is updated
    while true
        n = input('Position: ');
        j = rem(n-1,3)+1;
        i = floor((n-1)/3)+1;
        if length(n)==1 && rem(n,1)==0 && n>=1 && n<=9 || G(i,j)==' '
            break
        end
    end
    G(i,j) = player;
end


function f = score(G)
    % Return a score of either:
    % -1 - X wins
    % +1 - O wins
    %  0 - Nobody has won yet
    winner = @(G,c) any(all(G==c,1)) || any(all(G==c,2)) || all(diag(G)==c) || all(diag(flipud(G))==c);
    f      = winner(G,'O') - winner(G,'X');
end


function G = computer(G,player)
    % Update the board with the best move
    [~,ii] = search(G, player); % Find where to draw the O/X
    G(ii)  = player;            % Add O/X to the selected location
end


function [f,ii] = search(G, player)
    % Recursive function for finding the best move.
    % This assumes perfect play on both sides.  The objective function
    % could be modified to account for imperfect play in the human.

    ii = [];       % Needs to be defined
    f  = score(G); % Is it a winning/losing position
    if f == 0 && ~isdraw(G)

        % Define "best" for this player, and set up the next player
        if player=='X'
            best        = @min;
            next_player = 'O';
        else
            best        = @max;
            next_player = 'X';
        end

        indices = find(G==' ');
        if ~isempty(indices)
            funvals = zeros(size(indices));
            indices = indices(randperm(length(indices))); % Random permutation to make less predictable
            % Loop over available moves and find the best score
            for i = 1:length(indices)
                G(indices(i)) = player;
                funvals(i)    = search(G, next_player); % RECURSION
                G(indices(i)) = ' ';
            end
            [f,i] = best(funvals);
            ii    = indices(i);
        end
    end
end
```
