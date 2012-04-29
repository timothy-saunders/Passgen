function [pass_word] = password()
%Initiates a function which

n = input('How many different passwords do you need?');

z = input('Enable variable length passwords? Yes[1], No[0]');

if z == 1
    a = input('what is the maximmum length of the password that you would like?');
    b = input('What is the minimum length of the password that you would like?');
    pass_word = singpass(n, z, a, b);
else
    pass_word = singpass(n, z, 8, 8);
end


function [passw] = singpass(n, z, a, b)
% produces a password of 8 symbols long, n(number of passwords), z(variable
% length 1=yes), a(max length), (min length)

passw = cell(n,1);
checks = input('Would you like to check for upper case, lower case and digits? Y[1] N[0] (WARNING! This increases time taken considerably)');
for k = 1:n
    g = 1;
    
    if z == 1
        c = randi((a-b)+1);
        %genereate an array of random values
        pass = randi([48,122],1,(b+c));
        
        %adjust the values for each random integer if the corresponding
        %decimal does not correspond to an alphanumeric on the ascii table.
        while g <= length(pass)
             if (pass(g) >= 58 && pass(g) <= 64) || pass(g) >= 91 && pass(g) <= 96
                 pass(g) = randi([48,122]);
             else
                 g = g + 1;
             end
        end
    else
        pass = randi([48,109],1,8);
        while g <= 8
             if (pass(g) >= 58 && pass(g) <= 64) || pass(g) >= 91 && pass(g) <= 96
                 pass(g) = randi([48,122]);
             else
                 g = g + 1;
             end
        end
    end
    
    if pass(1) <= 64 || pass(1) >= 91 && pass(1) <= 96
    pass(1) = randi([65,90],1,1);
    end
    
       
    if checks == 1
        passed = check(pass);
        if passed == 1
            passw{k} = ('Invalid Password');
            
        else
            %change ascii values back to symbols
            passw{k} = num2str(char(pass));
        end
    else
        %change ascii values back to symbols
        passw{k} = num2str(char(pass));
    end
end



function [passv] = check(pass)
%checks a password for its validity

v = 1;
number = 0;
lower_case = 0;
upper_case = 0;
while v < length(pass)
    if pass(v) >= 48 && pass(v) <= 57
        number = number + 1;
        v = v + 1;
    elseif pass(v) >= 65 && pass(v) <= 90
        upper_case = upper_case + 1;
        v = v + 1;
    elseif pass(v) >= 97 && pass(v) <= 122
        lower_case = lower_case + 1;
        v = v + 1;
    end
end
if number == 0 || lower_case == 0 || upper_case == 0
    passv = 1;
else
    passv = 0;
end