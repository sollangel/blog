use BlockDist, HPCCProblemSize;
config type elemType = real;
config const m = computeProblemSize(numArrays=3, elemType),
         alpha = 3.0;
proc main() {
  const ProblemSpace = {1..m} dmapped Block(boundingBox={1..m});
  var A, B, C: [ProblemSpace] elemType;
  B = 2.0;
  C = 1.0;
  A = B + alpha * C;
}
