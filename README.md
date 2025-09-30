package com.example.calculator_sep;

import android.os.Bundle;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        EdgeToEdge.enable(this);

        setContentView(R.layout.activity_main);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main),
                (v, insets) -> {
                    Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
                    v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
                    return insets;
                });
    }

    private void checkForPowerof() {
        ArrayList<Integer> indexOfPowers = new ArrayList<>();

        for (int i = 0; i < workings.length(); i++) {
            if (workings.charAt(i) == '^') {
                indexOfPowers.add(i);
            }
        }

        formula = workings;
        tempFormula = workings;

        for (int i = 0; i < indexOfPowers.size(); i++) {
            changeFormula(indexOfPowers.get(i));
        }

        formula = tempFormula;
    }

    private void changeFormula(int index) {
        String numberLeft = "";
        String numberRight = "";

        for (int i = index - 1; i >= 0; i--) {
            if (isNumeric(workings.charAt(i))) {
                numberLeft = workings.charAt(i) + numberLeft;
            } else {
                break;
            }
        }

        for (int i = index + 1; i < workings.length(); i++) {
            if (isNumeric(workings.charAt(i))) {
                numberRight = numberRight + workings.charAt(i);
            } else {
                break;
            }
        }

        String original = numberLeft + "^" + numberRight;
        String changed = "Math.pow(" + numberLeft + "," + numberRight + ')';

        tempFormula = tempFormula.replace(original, changed);
    }
}
